# AI Product Bootstrap

If you've been using AI agents to build products, you've probably hit this: the first session goes great, the second is okay, and by the third the agent has forgotten your architecture, your files are all over the place, and you're spending more time re-explaining context than actually making progress.

I kept running into this, so I wrote a guide — a single markdown file you hand to the agent at the start of a project. It asks you about what you're building, then sets up a folder structure where each folder has a clear owner, clear rules about what goes in it, and pointers to what the agent should read before working there.

It works with Claude, ChatGPT, Gemini, Copilot, Cursor — really any agent that can read a file.

## Usage

1. Drop [`bootstrapping-guide.md`](bootstrapping-guide.md) into a conversation with your AI agent
2. Tell it what you're building
3. Answer a few questions about your project
4. The agent generates a structure like this:

```
0A-ceo/                  # Dashboard, priorities, roadmap
1A-vision/               # Problem, goals, personas
2A-research/             # Ecosystem, competitors
3A-requirements/         # Scope, MVP definition
4A-design/               # Interaction model, wireframes
4B-architecture/         # System design, tech stack
5A-specs/                # Feature specifications
5B-quality/              # Test strategy, edge cases
6A-build-plan/           # Implementation plan
7A-app/                  # Source code
AA-journal/              # Session logs
AB-decisions/            # Decision records
```

The number of folders depends on your project. A small library might only need 7. A bigger product could have 16. The guide figures this out with you.

Each folder gets a governance file that tells the agent what belongs there, what doesn't, and where to put things instead. So next session, the agent reads those files first and actually has context — instead of you having to re-explain everything.

## The three ideas behind it

**Hats** — The agent picks a role before working. When it's wearing the Product Owner hat, it thinks about what to build and for whom. When it's wearing the Developer hat, it thinks about how to build it. This stops the thing where the agent mixes up "should we build this feature" with "here's the database schema" in the same breath.

**Tiers** — Folders are numbered by how much their decisions affect everything else. Vision is tier 1 — if that changes, everything downstream changes too. App code is tier 7 — changes stay local. The practical effect is you figure out the important stuff first, which seems obvious but AI agents will happily jump to writing code before you've even agreed on what the product does.

**Discussion roadmap** — The folders start empty. The roadmap is a plan for filling them in across multiple sessions — vision first, then users, then research, then scope, then design, then architecture, then code. Each phase has a clear goal and clear outputs.

## Examples

The guide includes complete examples for different project sizes:

- Open source library — 4 roles, 7 folders
- CLI tool — 5 roles, 10 folders
- SaaS platform — 10 roles, 16 folders
- Analytics product — 6 roles, 10 folders

It also has templates for every governance file — dashboards, role profiles, decision records, session logs, etc.

## Contributing

If you run into a project type that doesn't fit, open an issue or PR against `bootstrapping-guide.md`.

This repo is just the guide and this README — that's intentional.

## Support

<a href="https://buymeacoffee.com/ThanhWilliamLe">
  <img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" />
</a>

<!-- Uncomment when ready:
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?style=for-the-badge&logo=github-sponsors)](https://github.com/sponsors/ThanhWilliamLe)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-F16061?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/ThanhWilliamLe)
-->

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=ThanhWilliamLe/ai-product-bootstrap&type=Date)](https://star-history.com/#ThanhWilliamLe/ai-product-bootstrap&Date)

## License

[MIT](LICENSE)
