<div align="center">

# AI Product Bootstrap

**A structured way to build products with AI agents. From idea to product, built to last.**

Give this guide to your AI agent. The agent sets up organized folders and a plan for what to figure out first.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/ThanhWilliamLe/ai-product-bootstrap/pulls)

</div>

---

For developers and founders who use AI agents to build products and are tired of re-explaining context each session.

Works with Claude, ChatGPT, Gemini, Copilot, Cursor. Anything that reads markdown.

## How it works

```mermaid
flowchart LR
    A["📄 You give the guide\nto your AI agent"] --> B["💬 Agent asks about\nyour project"]
    B --> C["🏗️ Agent scaffolds\nfolders + governance"]
    C --> D["🗺️ Agent generates\na discussion roadmap"]
    D --> E["🔄 Next session:\nagent reads governance\nand picks up where\nyou left off"]
```

1. Drop [`bootstrapping-guide.md`](bootstrapping-guide.md) into a conversation
2. Describe what you're building
3. Answer a few questions
4. The agent builds the project scaffold:

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

Each folder has a governance file that says what belongs and where to redirect mistakes. Next session, the agent reads these first and picks up your context.

The number of folders scales to your project. A weekend library might get 7, a team SaaS product 16.

## What's behind it

Three ideas drive the guide:

**Hats.** The agent declares a role before doing anything. Product Owner thinks about *what* to build, Developer thinks about *how*. You get focused answers instead of a mix of strategy and implementation.

**Tiers.** Folders are numbered by how much their decisions ripple outward. You work top-down. AI agents won't do that on their own.

```mermaid
flowchart LR
    T1["Tier 1 · Vision\nchanges ripple everywhere"] --> T4["Tier 4 · Architecture\nchanges ripple locally"] --> T7["Tier 7 · Code\nchanges stay here"]
```

**Discussion roadmap.** Folders start as empty stubs. You fill them across sessions, each phase with a goal and deliverables.

```mermaid
flowchart LR
    R1["Phase 1\nVision"] --> R2["Phase 2\nUsers"] --> R3["Phase 3\nScope"] --> R4["Phase 4\nDesign"] --> R5["Phase 5\nBuild"] --> R6["Phase 6\nValidate"]
```

**Autonomous continuation.** The dashboard has a machine-readable "Next Action" table. Agents pick up work without you prompting. After build, a validation loop spawns sub-agents as different users to test scenarios until 97% pass rate.

> [!TIP]
> You don't need to understand any of this before starting. The agent walks you through it.

## Examples

See walkthroughs for different project sizes:

| Project type | Roles | Folders |
|-------------|-------|---------|
| Open source library | 4 | 7 |
| CLI tool | 5 | 10 |
| SaaS platform | 10 | 16 |
| Analytics product | 6 | 10 |

Plus templates for governance files: dashboards, role profiles, decision records, session logs.

## Get started

```bash
# Clone and use
git clone https://github.com/ThanhWilliamLe/ai-product-bootstrap.git

# Or grab the guide directly
curl -O https://raw.githubusercontent.com/ThanhWilliamLe/ai-product-bootstrap/main/bootstrapping-guide.md
```

Then open it in a conversation with your AI agent and say what you're building.

## Contributing

If you run into a project type that doesn't fit, open an issue or PR against `bootstrapping-guide.md`.

This repo contains the guide and this README. Nothing else.

## Support

[![Ko-fi](https://img.shields.io/badge/Ko--fi-F16061?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/ThanhWilliamLe)

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=ThanhWilliamLe/ai-product-bootstrap&type=Date)](https://star-history.com/#ThanhWilliamLe/ai-product-bootstrap&Date)

## License

[MIT](LICENSE)
