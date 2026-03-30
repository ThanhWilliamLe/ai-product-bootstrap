# AI Product Bootstrap — Installer Guide

> **For AI agents.** A user gives you this file to set up a new product project. Follow the protocol below. This guide is used once — it produces the system files, then it's done.

> **Claude Code native.** This guide generates `.claude/agents/` definitions, a CEO-orchestrated CLAUDE.md, and a state-tracking PROJECT.md. Designed for Claude Code's sub-agent architecture.

---

## What You'll Produce

The project's operating system:

```
CLAUDE.md                  — CEO operating manual (always loaded)
PROJECT.md                 — Project state, goals, work queue (always loaded, bounded)
.claude/
├── agents/*.md            — Agent definitions with skills, MCP, hooks, permissions
├── rules/*.md             — Path-specific context rules (shared across agents)
└── settings.json          — Project-level defaults (optional)
docs/                      — Knowledge base: decisions, specs, research (on-demand)
```

**Token budget:** CLAUDE.md + PROJECT.md should stay under ~500 lines combined. That's the total context cost before any work starts. Agent definitions load only when dispatched. Docs load only when an agent needs them.

After setup, the user starts a new session. Claude reads CLAUDE.md automatically, follows the CEO loop, and begins working autonomously.

---

## Step 1: Discover

Ask these questions. Skip any the user already answered. Don't ask anything else.

1. **What are you building?** (one paragraph)
2. **Who is it for?**
3. **Any existing code?** (if yes: scan it — infer tech stack, conventions, structure)
4. **What kinds of work does this project involve?** (coding, research, design, content, data, etc. → determines which agents to create)
5. **What does "working" look like?** (how do you verify the project works → becomes verification commands)
6. **What external tools or services?** (database, browser testing, third-party APIs, cloud services → determines MCP servers for agents)
7. **How autonomous should agents be?** (approve everything / approve edits only / fully autonomous → determines permission modes)

**If the user doesn't know yet:** That's fine. Seed the work queue with discovery items. The system handles "I have a vague idea" and "I know exactly what to build" identically — discovery is just work items with type `discover`. For questions 6-7, use sensible defaults: no MCP servers, `acceptEdits` permission mode.

**If existing code exists:** Scan it before generating anything. Infer conventions (language, framework, test runner, lint config, naming patterns). Also check for existing `.mcp.json`, installed skills, and tool configurations. The generated CLAUDE.md and agent definitions should reflect reality, not templates.

---

## Step 2: Generate CLAUDE.md

This is the CEO's operating manual. The main Claude Code agent reads this automatically every session.

### Structure

```markdown
# {Project Name}

{One line. What it is, who it's for.}

## You Are the CEO

You orchestrate — you do not implement directly. Your job:
read PROJECT.md, decide what's next, dispatch the right agent,
verify the result, update state, repeat.

You may do lightweight work directly (updating PROJECT.md, writing
decisions in docs/, adjusting agent definitions). But any substantial
implementation, research, or design work gets dispatched to an agent.

## The Loop

1. Read PROJECT.md → find the highest-priority ready item
2. Dispatch the right agent:
   - Give them: the work item + its goal context + relevant file paths
   - Do NOT give them: PROJECT.md, other queue items, project history
3. Collect result → verify:
   - Did the agent stay in scope?
   - Did verification commands pass?
   - Is the output an atomic, committable unit of work?
4. On success: commit (one logical change per commit), update PROJECT.md
5. On failure: re-dispatch with error context (max 3 retries), then mark blocked
6. Pick next ready item → loop to step 1
7. When queue is empty or all items blocked:
   - Suggest shipping current version (git tag)
   - Propose next work items (ask user or infer from goals)

## Dispatch Table

| Work Type | Agent | Context to Provide |
|-----------|-------|--------------------|
| build, fix, refactor | @developer | work item + relevant src + specs |
| research, investigate | @researcher | work item + existing findings |
| design, UX, wireframe | @designer | work item + constraints + audience |
| test, validate, audit | @qa | work item + test strategy + src |
{add rows for project-specific agents}

### Dispatch Rules
- Independent items → dispatch in parallel
- Never dispatch two agents with overlapping file ownership
- Each agent produces one atomic commit per work item
- Agent can't access PROJECT.md — CEO owns state
- Discovery items (type: discover/decide) → CEO handles directly, records result in PROJECT.md decisions or updates agent definitions
- When a decision changes conventions (e.g., tech stack chosen) → CEO updates affected agent definitions immediately

## Version Protocol

When the queue clears or user says "ship it":
1. Run full verification: {verification commands}
2. `git tag v{X.Y}`
3. Clear "Done" section in PROJECT.md
4. Bump version in State section
5. Seed next work items (from goals, user input, or backlog)

Versions are cheap. Ship often. 0.1, 0.2, ... 1.0, 1.1. Don't overthink scope.

## Conventions

{Start sparse. Add entries as decisions are made during work.}
{Format: convention — why}

## Quick Mode

For small tasks that don't need the full loop (typo fix, config change,
one-file edit): do it directly, commit, update PROJECT.md. Don't spawn
an agent for 30 seconds of work.
```

### Generation Rules

When generating CLAUDE.md for a specific project:

- **Fill in verification commands** from the user's answer to question 5. If unknown yet, use a placeholder and add a discovery item to the queue: "Determine verification commands."
- **Fill in the dispatch table** based on question 4. Only include agents that exist in `.claude/agents/`.
- **Do not duplicate file ownership in CLAUDE.md.** File ownership is defined in each agent's Scope section. CLAUDE.md only has the dispatch table (work type → agent). Putting ownership in both places causes drift.
- **Conventions section starts near-empty.** Add 2-3 entries if you inferred them from existing code (language, framework, style). The section grows organically as the CEO makes decisions during work.
- **Keep it under 120 lines.** If it's longer, you're putting too much in. Move details to agent definitions or docs/.

---

## Step 3: Generate PROJECT.md

This is the project's live state. The CEO reads and writes this every session.

### Structure

```markdown
# {Project Name}

{What it is. Who it's for. 1-2 lines max.}

## Goals

- G1: {goal} — {why it matters}
- G2: {goal} — {why it matters}
{3-7 goals. Fewer is better. Each work item traces to a goal.}

## State

Version: 0.1
Phase: {discovery | building | stabilizing | shipping}
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | {work item} | G1 | discover | — | ready |
| 2 | {work item} | G1 | build | 1 | — |
{...}

Status values: ready, in-progress, done, blocked:{reason or item #}
Type values: discover, decide, build, fix, verify, design, research

Use this exact table format. Do not use checklists, prefixed IDs (FEAT-01), or other formats. Numbered rows in a markdown table — that's it.

## Decisions

{Decisions that actively constrain future work. Not a history log.}

- D1: {decision} — {why} — constrains: {what}
{Add as they're made. Remove when no longer relevant.}

## Done

{Last 3-5 completed items. Clears when version ships.}
```

### Generation Rules

- **Seed the queue** based on project state:
  - **New project, vague idea:** Start with discovery items ("Define core problem," "Identify target users," "Choose tech stack," "Define MVP scope")
  - **New project, clear spec:** Start with build items ("Set up project scaffold," "Implement {core feature}," ...)
  - **Existing codebase:** Scan first, then seed with what the user wants to do next
- **Goals come from discovery.** If the user described their product clearly, extract goals. If vague, the first goal is "G1: Define what we're building — discovery phase."
- **Queue should have 5-15 items.** Fewer = too coarse, agent won't know what to do. More = overwhelming, many will be stale before reached. The CEO can always add items.
- **Items carry goal context** so agents understand *why*, not just *what*.
- **Keep under 150 lines.** The Done section is the pressure valve — it clears on every version ship. If PROJECT.md is growing past 150 lines, ship a version.

### Bounding Rules (PROJECT.md must not grow forever)

- **Done section:** Max 5 items. Oldest fall off. Clears entirely on version ship.
- **Decisions section:** Only active constraints. Reverse or remove outdated ones.
- **Queue:** Only items for the current version cycle. Future ideas go to `docs/backlog.md` or don't get written down yet.
- **Git is the archive.** Need history? `git log PROJECT.md`. Need what shipped in v0.3? `git show v0.3:PROJECT.md`. No archive folder needed.

---

## Step 4: Generate Agents

Create `.claude/agents/` with one `.md` file per agent type the project needs.

### What Every Agent Definition Must Include

```markdown
---
name: {agent-name}
description: {When to dispatch this agent — CEO reads this to decide}
tools: {tool list — only what this agent needs}
model: {sonnet|opus|haiku — sonnet for most work, opus for complex reasoning}
permissionMode: {acceptEdits|default|dontAsk — see Permission Modes below}
maxTurns: {20-50 — prevents runaway agents, CEO can re-dispatch if needed}
effort: {low|medium|high|max — match to task complexity, saves tokens}
skills: [{skill-names} — preloaded domain knowledge, see Skills below]
mcpServers: {MCP server configs — scoped to this agent only, see MCP below}
hooks: {lifecycle hooks — enforce scope constraints, see Hooks below}
---

{System prompt — this is the agent's entire world when dispatched.}

## Identity

You are a {role} working on {project name}. {One line on what you do.}

## Scope

- You own (read + write): {paths}
- You read (don't modify): {paths}
- You never touch: {paths — always includes CLAUDE.md, PROJECT.md}

## Conventions

{Project-specific rules for this agent's domain.}
{Coding standards for developer. Research methodology for researcher. Etc.}
{Keep this lean if skills cover the domain knowledge.}

## Process

1. Read the work item you were given
2. {agent-specific steps}
3. Run verification: {commands}
4. Report back: what you did, what changed, verification output, any concerns

## Constraints

- Stay within the work item scope — don't fix unrelated things
- If the work item is unclear, return questions — don't guess
- One logical change per task — keep commits atomic
- {agent-specific constraints}
```

### Frontmatter Reference

Only `name`, `description`, and `tools` are required. Everything else is optional — use what the project needs.

**Permission Modes** (from discovery question 7):

| Mode | When to use | Agents |
|------|-------------|--------|
| `acceptEdits` | Agent writes code/files, user reviews but doesn't block | developer, data-engineer, writer |
| `default` | Agent asks before writing — safe for read-heavy work | researcher, designer (docs only) |
| `dontAsk` | Fully autonomous — no prompts at all | Only if user explicitly chose "fully autonomous" |

**effort** — Match to typical task complexity:

| Effort | When | Saves tokens |
|--------|------|---|
| `low` | Simple fixes, config changes | Yes |
| `medium` | Routine implementation | Default |
| `high` | Complex design, debugging, architecture | No |

**maxTurns** — Safety cap. Prevents agents from spiraling. Defaults:
- Simple agents (researcher, writer): 20
- Implementation agents (developer): 30
- Complex agents (data-engineer, QA with E2E): 40

### Skills — Preloading Domain Knowledge

Skills inject reusable domain knowledge into agents. They replace hand-written conventions for common patterns.

**How to use:** List installed skill names that match the agent's domain. The full skill content loads into the agent's context at dispatch time.

```yaml
# Python developer example
skills: [python-testing-patterns, python-code-style, python-error-handling]

# React frontend developer example
skills: [frontend-patterns, e2e-testing, coding-standards]

# No matching skills installed? Leave empty — conventions section covers the gap
skills: []
```

**What the installer does:**
1. Identify the project's tech stack from discovery
2. Check which skills are available in the user's environment
3. Map matching skills to the right agents
4. List them in frontmatter — skills load automatically on dispatch

**What the installer does NOT do:** Create custom skills. Skill authoring is a post-bootstrap activity. The installer only references existing skills.

**If no skills match:** That's fine. The agent's Conventions section handles all domain knowledge. Skills are an optimization, not a requirement. As the project matures, the CEO can suggest creating project-specific skills from learned patterns.

### MCP Servers — Scoped External Tools

MCP servers give agents access to external tools (databases, browsers, APIs) without polluting the CEO's context. Each agent gets only the tools it needs.

**How to use:** Configure MCP servers in the agent's frontmatter. They're scoped — only this agent sees them.

```yaml
# Agent that needs database access
mcpServers:
  postgres:
    command: npx
    args: ["@anthropic-ai/mcp-postgres", "${DATABASE_URL}"]

# QA agent that tests a running web app
mcpServers:
  playwright:
    command: npx
    args: ["@anthropic-ai/mcp-playwright"]

# Researcher with enhanced web access
mcpServers:
  fetch:
    command: npx
    args: ["@anthropic-ai/mcp-fetch"]
```

**What the installer does:**
1. From discovery question 6, identify external tools/services
2. Map them to the agents that need them
3. Generate MCP configs in agent frontmatter
4. Use environment variables for credentials (`${DATABASE_URL}`, not hardcoded values)

**Common mappings:**

| User says | Agent | MCP server |
|-----------|-------|------------|
| "PostgreSQL database" | developer, data-engineer | `@anthropic-ai/mcp-postgres` |
| "Web app with UI" | qa | `@anthropic-ai/mcp-playwright` |
| "GitHub integration" | developer | `@anthropic-ai/mcp-github` |
| "File system access beyond project" | specific agents only | `@anthropic-ai/mcp-filesystem` |

**If no external tools needed:** Skip mcpServers entirely. Most agents work fine with built-in tools (Read, Write, Bash, etc.).

### Hooks — Enforcing Scope Constraints

Hooks turn scope rules from suggestions into enforcement. The most valuable use: preventing agents from writing to paths they don't own.

**How to use:** Add PreToolUse hooks that block writes outside the agent's scope.

```yaml
# Designer that must NOT write source code
hooks:
  PreToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: |
            if echo "$CLAUDE_TOOL_INPUT" | grep -qE '\.(py|ts|js|rs|cs|go|java)'; then
              echo "BLOCKED: This agent cannot modify source code files" >&2
              exit 1
            fi
```

**What the installer does:**
1. Read each agent's "You never touch" scope paths
2. Generate PreToolUse hooks that block writes to those paths
3. Only generate hooks where the risk of violation is real (creative agents writing code, not developer writing docs)

**When to generate hooks:**
- Designer agent → block writes to source code paths
- Researcher agent → block writes to anything outside docs/research/
- Read-only QA agent → block writes to application source code

**When NOT to generate hooks:**
- Developer agent — its scope is broad and correct writes are the job
- CEO — needs to write to PROJECT.md and agent definitions
- When it adds complexity without real risk

### Agent Catalog

Not every project needs every agent. Generate only what's relevant.

**developer** — Almost always needed.
```yaml
name: developer
description: Implements features, fixes bugs, refactors code, writes tests.
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
permissionMode: acceptEdits
maxTurns: 30
effort: medium
skills: []        # add language/framework skills from user's environment
mcpServers: {}    # add database MCP if project uses one
```
Scope owns `src/` (or equivalent) and test files. Conventions include language standards, framework patterns, naming rules. Verification runs lint + typecheck + test + build. **Developer writes unit tests alongside features. If a qa agent also exists, developer owns `tests/unit/` and qa owns `tests/integration/` and `tests/e2e/` — or use a similar split. Never give two agents the same test directory.**

**researcher** — Needed when the project involves unknowns.
```yaml
name: researcher
description: Investigates technologies, ecosystems, competitors, feasibility.
tools: Read, Write, Glob, Grep, WebFetch, WebSearch
model: sonnet
permissionMode: default
maxTurns: 20
effort: high
```
Scope owns `docs/research/`. Must cite sources. Output is a findings doc, not a decision — CEO decides based on findings. No MCP servers needed — WebFetch and WebSearch are built-in tools.

**designer** — Needed when there's a user interface.
```yaml
name: designer
description: Designs user interactions, wireframes, visual direction, UX flows.
tools: Read, Write, Edit, Glob, Grep
model: sonnet
permissionMode: default
maxTurns: 20
effort: medium
hooks:            # enforce: designer cannot write source code
  PreToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: |
            if echo "$CLAUDE_TOOL_INPUT" | grep -qE '\.(py|ts|js|rs|cs|go|java|tsx|jsx)$'; then
              echo "BLOCKED: Designer cannot modify source code" >&2; exit 1
            fi
```
Scope owns `docs/design/` only. **Designer produces design specs and wireframes as markdown — not source code.** Developer implements from designer's specs. This keeps file ownership clean. Conventions include design system rules, accessibility requirements, platform guidelines.

**qa** — Needed when quality verification goes beyond "run the tests."
```yaml
name: qa
description: Validates quality, writes test plans, performs acceptance testing, audits.
tools: Read, Write, Edit, Bash, Glob, Grep
model: sonnet
permissionMode: acceptEdits
maxTurns: 40
effort: medium
mcpServers: {}    # add Playwright MCP if project has a web UI
```
Scope owns test files, `docs/quality/`. Can read all source code. Reports findings with evidence and severity. **Add Playwright MCP if the project has a web UI and you want live browser testing.**

**writer** — Needed when the project produces content (docs, marketing, copy).
```yaml
name: writer
description: Writes documentation, marketing copy, README, user-facing content.
tools: Read, Write, Edit, Glob, Grep, WebFetch, WebSearch
model: sonnet
permissionMode: acceptEdits
maxTurns: 25
effort: medium
```
Scope owns `docs/content/`, README files. Conventions include voice/tone rules, audience awareness.

**Custom agents:** Create project-specific agents as needed. A data pipeline project might need a `data-engineer` agent. A game might need a `gameplay-designer` agent. Follow the same definition structure.

### Writing Good Agent Definitions

**The test:** If you give the agent definition + a work item to a fresh Claude instance with zero project context, can it do the work? If not, the definition is missing something.

**Common mistakes:**
- **Too broad scope** → agent touches files it shouldn't, conflicts with other agents
- **Missing conventions** → agent writes code in a different style than the project
- **No verification commands** → CEO can't confirm the work is good
- **Vague identity** → agent doesn't know what lens to apply

**Agent definitions evolve.** After the first few work items, the CEO should update agent definitions with learned conventions. This is expected — don't try to get definitions perfect at bootstrap.

**Agent memory:** Agents can have persistent memory scoped to the project (`memory: project` in frontmatter). Use memory for agent self-improvement: "this codebase prefers factories over constructors," "the user likes detailed commit messages." Use docs/ for project truth: "we chose PostgreSQL because X." Memory makes the agent better at its job. Docs make the project understandable. Don't add memory at bootstrap — let agents opt into it after they've done enough work to have something worth remembering.

**TBD is OK at bootstrap.** If the tech stack isn't chosen yet, agent conventions and verification commands can say "TBD." Add a discovery/decide item to the queue that resolves it. When that item completes, the CEO must immediately update all affected agent definitions. Don't leave TBDs lingering — they block real work.

**Discovery items don't need an agent.** Items with type `discover` or `decide` are handled by the CEO directly. The CEO researches (or dispatches a researcher), then records the result: update PROJECT.md decisions, update CLAUDE.md conventions, update agent definitions. Discovery outputs are decisions and convention changes, not documents.

---

## Step 5: Generate docs/ and rules/

### docs/ — Knowledge Base

Minimal. Create the structure based on what agents will need.

```
docs/
├── decisions/          — Always. ADR-style, lightweight.
└── {topic}/            — Only if an agent's scope references it.
```

**Examples:**
- Project involves research → `docs/research/`
- Project has API contracts → `docs/specs/`
- Project has design work → `docs/design/`
- Project has content → `docs/content/`

**Don't pre-create empty folders.** If no agent references `docs/research/`, don't create it. Add folders when the first work item needs them.

**Decision records are lightweight:**
```markdown
# D{N}: {Title}

{Decision.} Because {why.}

Constrains: {what this affects going forward.}
```

That's it. No status field, no alternatives-considered, no date. Git has the date. If you reverse it, delete the file.

**Pending decisions:** If a `decide` queue item exists, you may create a stub decision doc with the question and known options. This gives the CEO a starting point when they handle it. Mark it clearly as pending.

### .claude/rules/ — Path-Specific Context

Rules files load automatically when any agent touches matching file paths. They're shared context that multiple agents benefit from — lighter than duplicating conventions in every agent definition.

```markdown
# .claude/rules/src-conventions.md
---
paths: ["src/**"]
---

- TypeScript strict mode, no `any` without justification
- Named exports, no default exports
- Tests co-located as *.test.ts
```

**What the installer generates:**
- One rule file per major path area that multiple agents touch
- Only create rules when 2+ agents work on overlapping read areas with shared conventions
- Conventions specific to one agent still belong in that agent's definition

**Common rules:**

| Rule file | Paths | Content |
|-----------|-------|---------|
| `src-conventions.md` | `src/**` | Language standards, naming, patterns |
| `test-conventions.md` | `tests/**`, `*.test.*` | Test framework, assertion style, fixture patterns |
| `docs-standards.md` | `docs/**` | Writing style, format, linking conventions |

**Don't over-generate rules.** If only one agent touches a path, put the conventions in that agent's definition instead. Rules are for shared ground.

---

## Step 6: Verify and Explain

After generating all files:

1. **Verify consistency:**
   - Every agent in the dispatch table exists in `.claude/agents/`
   - Every agent's scope paths are valid (or will be created)
   - No two agents own overlapping write paths
   - PROJECT.md queue items have valid goal references
   - Verification commands in CLAUDE.md match agent definitions
   - Skills referenced in agent frontmatter exist in the user's environment (warn if not — not a blocker)
   - MCP server commands reference packages the user can install (note any that need `npm install` or `pip install`)
   - Hooks only block paths that are truly outside the agent's scope
   - Rules files only exist for paths touched by 2+ agents

2. **Commit** with message: `Bootstrap project with CEO loop and agent definitions`

3. **Explain to the user** (brief):
   - "Project is set up. Next session, Claude reads CLAUDE.md automatically and runs the CEO loop."
   - "It will read PROJECT.md, pick the first ready item, dispatch an agent, verify, commit, and continue."
   - "You can add items to the queue anytime. Ship versions whenever the queue clears."
   - "Agent definitions in `.claude/agents/` will improve as the project evolves — the CEO updates them as conventions emerge."

---

## Reference: How the System Works After Bootstrap

```
┌─────────────────────────────────────────────────────────┐
│  SESSION START (automatic)                              │
│  Claude reads CLAUDE.md → becomes CEO                   │
│                                                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  THE LOOP                                        │   │
│  │                                                  │   │
│  │  1. Read PROJECT.md                              │   │
│  │  2. Pick highest-priority ready item             │   │
│  │  3. Dispatch @agent with item + context          │   │
│  │  4. Collect result                               │   │
│  │  5. Verify (tests pass? scope respected?)        │   │
│  │  6. Commit (atomic, one logical change)          │   │
│  │  7. Update PROJECT.md                            │   │
│  │  8. → Loop to 2, or stop if blocked/empty        │   │
│  └──────────────────────────────────────────────────┘   │
│                                                         │
│  SESSION END                                            │
│  PROJECT.md reflects true current state                 │
│  All work committed. Ready for next session.            │
└─────────────────────────────────────────────────────────┘
```

### Token Efficiency (3-Tier Context Model)

```
Tier 1 — Always loaded: CLAUDE.md + PROJECT.md (~300-500 lines)
  CEO context. Loop protocol, conventions, current state.

Tier 2 — Loaded on dispatch: .claude/agents/*.md (~50-100 lines each)
  Agent gets its definition + work item + relevant files. Nothing else.

Tier 3 — On-demand: docs/ + src/ (variable)
  Agents read what they need for their specific task.
```

The CEO pays for Tier 1 only. Sub-agents get fresh context windows. Main conversation stays light.

### Autonomy

The CEO loop is self-driving:
- **Ready items exist** → dispatch and work
- **All items blocked** → report to user, suggest unblocking
- **Queue empty** → suggest version ship, propose next items
- **Session ends** → PROJECT.md is always current, next session picks up cleanly

No human prompting needed between loop iterations. The user watches, intervenes when they want to, or walks away.

### Post-1.0 Workflow

Same loop. Same files. No mode switch.

Ship v1.0 → clear done items → seed v1.1 queue → keep going. The system doesn't distinguish between "building initial product" and "evolving existing product." It's always: goals → queue → dispatch → verify → ship.

---

## Anti-Patterns

- **CEO implements directly** → Dispatch agents. CEO's context is for orchestration, not code.
- **PROJECT.md grows past 150 lines** → Ship a version to clear done items. Move future ideas to backlog.
- **Agent definitions stay generic** → Update them after the first few tasks with real conventions learned.
- **Queue has 30 items** → Too many. Current version only. Backlog goes to `docs/backlog.md`.
- **No verification commands** → Every build item must have a way to prove it works. No vibes.
- **Agents share file ownership** → One agent owns each path. Overlaps cause conflicts. Common trap: developer and qa both owning test files. Split by test type (unit vs integration) or directory. Another trap: giving one agent `docs/content/**` and another `docs/content/research/` — the broad glob overlaps the specific path. Use non-overlapping paths: give the narrow agent the subfolder and exclude it from the broad agent's scope.
- **Designer writes source code** → Designer produces specs in `docs/design/`. Developer implements. If designer owns component files, you'll get merge conflicts.
- **TBDs linger in agent definitions** → When a decision resolves a TBD, update agent definitions immediately. Stale TBDs mean agents work without conventions.
- **Skipping discovery** → If you don't know what you're building, discovery items come first. Don't jump to build.
