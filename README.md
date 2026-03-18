# AI Product Bootstrap

One markdown file. Give it to your AI agent. It scaffolds your entire project — folders, roles, governance, and a roadmap for what to discuss first.

Works with Claude, ChatGPT, Gemini, Copilot, Cursor, or whatever you use.

## Why

AI agents are great at writing code. They're terrible at organizing a project. Three sessions in and your files are everywhere, decisions are lost in chat history, and the AI jumps to code before you've figured out what you're actually building.

This guide makes the agent stop and think. It assigns roles (we call them "hats"), creates folders ordered by how much they affect everything else, writes governance rules so nothing gets misplaced, and builds a phased roadmap so you discuss vision before architecture and architecture before code.

## How it works

1. Drop [`bootstrapping-guide.md`](bootstrapping-guide.md) into a conversation
2. Describe what you're building
3. The agent asks a few questions, then scaffolds this:

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

Every folder gets a CLAUDE.md that says what goes there, what doesn't, and where to put things instead. The agent reads these at the start of each session so it actually remembers your project structure.

Folders scale to your project — a solo library might get 7, a team SaaS platform might get 16.

## The key ideas

**Hats** — The agent declares a role before working. Product Owner thinks about scope. Developer thinks about architecture. QA thinks about edge cases. No more blurring everything together.

**Tiers** — Folders are numbered by influence. Vision is tier 1 (changes ripple everywhere). App code is tier 7 (changes stay local). You discuss tier 1 before tier 7.

**Discussion roadmap** — The folders start empty. The roadmap tells you what to fill in and in what order, across multiple sessions.

## Examples in the guide

- Open source library — 4 roles, 7 folders
- CLI tool — 5 roles, 10 folders
- SaaS platform — 10 roles, 16 folders
- Analytics product — 6 roles, 10 folders

Full templates included for every file type.

## Contributing

Open an issue or PR against `bootstrapping-guide.md`. This repo is intentionally just one guide file and a README.

## Support

If this saved you time:

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
