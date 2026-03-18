# AI Product Bootstrap

You know the drill. You open Claude or ChatGPT, describe your idea, and the agent starts writing code immediately. By session three your repo is a mess — files dumped in random places, half your decisions buried in a chat you'll never scroll back to, and the AI has no idea what it built yesterday.

This is one markdown file that fixes that. You give it to your AI agent, tell it what you're building, and it sets up your entire project with clear roles, organized folders, and a step-by-step plan for figuring things out in the right order.

Works with Claude, ChatGPT, Gemini, Copilot, Cursor — anything that can read a file.

## What actually happens

1. Drop [`bootstrapping-guide.md`](bootstrapping-guide.md) into a conversation
2. Describe your project in a few sentences
3. The agent walks you through a few questions, then generates something like this:

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

Each folder gets governance rules — what belongs there, what doesn't, and where to redirect things that land in the wrong place. Next session, the agent reads these files first, so it picks up exactly where you left off instead of asking "what are we building again?"

Your project scales the structure. Solo weekend project? Maybe 7 folders. Team SaaS? Could be 16. The guide handles both.

## How it thinks

The guide introduces three ideas that make AI agents way more useful:

**Hats** — Before doing anything, the agent picks a role. Product Owner thinks about *what* to build. Developer thinks about *how*. QA thinks about what could break. You stop getting answers that mix strategy with implementation details.

**Tiers** — Folders are ordered by blast radius. Vision (tier 1) affects everything. Code (tier 7) affects only itself. So you nail down your vision before picking a tech stack, and pick your tech stack before writing specs. Sounds obvious, but left to their own devices, AI agents skip straight to tier 7.

**Discussion roadmap** — All those folders start as empty stubs. The roadmap is your plan for filling them in — one focused conversation at a time, in the right order, with clear deliverables for each phase.

## Included examples

The guide has full walkthroughs for different project sizes:

- Open source library — 4 roles, 7 folders
- CLI tool — 5 roles, 10 folders
- SaaS platform — 10 roles, 16 folders
- Analytics product — 6 roles, 10 folders

Plus ready-to-use templates for every file: dashboards, role profiles, decision records, session logs, all of it.

## Contributing

Found something that doesn't fit your project type? Open an issue or PR against `bootstrapping-guide.md`.

This repo is intentionally just one file and a README. That's the whole point.

## Support

If this helped you ship faster:

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
