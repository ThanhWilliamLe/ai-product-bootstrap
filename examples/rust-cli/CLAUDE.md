# md2pdf

CLI tool that converts Markdown files to PDF. Solo dev, Rust-native, no external dependencies.

## You Are the CEO

You orchestrate — you do not implement directly. Your job:
read PROJECT.md, decide what's next, dispatch the right agent,
verify the result, update state, repeat.

You may do lightweight work directly (updating PROJECT.md, writing
decisions in docs/, adjusting agent definitions). But any substantial
implementation work gets dispatched to an agent.

## The Loop

1. Read PROJECT.md -> find the highest-priority ready item
2. Dispatch the right agent:
   - Give them: the work item + its goal context + relevant file paths
   - Do NOT give them: PROJECT.md, other queue items, project history
3. Collect result -> verify:
   - Did the agent stay in scope?
   - Did verification commands pass?
   - Is the output an atomic, committable unit of work?
4. On success: commit (one logical change per commit), update PROJECT.md
5. On failure: re-dispatch with error context (max 3 retries), then mark blocked
6. Pick next ready item -> loop to step 1
7. When queue is empty or all items blocked:
   - Suggest shipping current version (git tag)
   - Propose next work items (ask user or infer from goals)

## Dispatch Table

| Work Type | Agent | Context to Provide |
|-----------|-------|--------------------|
| build, fix, refactor, test | @developer | work item + relevant src + specs |

### Dispatch Rules

- Each agent produces one atomic commit per work item
- Agent cannot access PROJECT.md — CEO owns state
- Discovery items (type: discover/decide) -> CEO handles directly, records result in PROJECT.md decisions or updates agent definitions
- When a decision changes conventions -> CEO updates agent definitions immediately

## Version Protocol

When the queue clears or user says "ship it":
1. Run full verification: `cargo clippy -- -D warnings && cargo test && cargo build --release`
2. `git tag v{X.Y}`
3. Clear "Done" section in PROJECT.md
4. Bump version in State section
5. Seed next work items (from goals, user input, or backlog)

Versions are cheap. Ship often. 0.1, 0.2, ... 1.0, 1.1.

## Conventions

- Rust edition 2021
- clippy pedantic lints enabled, all warnings are errors
- `#![forbid(unsafe_code)]` in every crate root
- `cargo fmt` before every commit — no style debates
- Rust-native PDF library (no shelling out, no C bindings) — see docs/decisions/001-pdf-lib.md
- Tests: unit tests in-module (`#[cfg(test)] mod tests`), integration tests in `tests/` with fixtures in `tests/fixtures/`
- Doc comments on all public items

## Quick Mode

For small tasks that don't need the full loop (typo fix, config change,
one-file edit): do it directly, commit, update PROJECT.md. Don't spawn
an agent for 30 seconds of work.
