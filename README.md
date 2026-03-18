# AI Product Bootstrap

**Give this guide to an AI agent. Get a fully governed project structure in minutes.**

A single-file protocol that turns any AI coding agent (Claude, GPT, Gemini, Copilot) into a product development consultant — one that scaffolds your entire project with role-based governance, tiered folder structures, and a phased discovery roadmap.

No frameworks. No dependencies. Just one `.md` file and a conversation.

---

## The Problem

You start a new project with an AI agent. Within 3 sessions you have:
- Files scattered everywhere with no ownership
- Architecture decisions buried in chat history
- No separation between "what to build" and "how to build it"
- The AI treating every question as a coding question

**This guide fixes that.** It teaches AI agents to think in roles, organize by influence, and plan before coding.

---

## What You Get

Hand [`bootstrapping-guide.md`](bootstrapping-guide.md) to your AI agent and answer a few questions about your project. The agent produces:

```
0A-ceo/                  # Coordination — dashboard, priorities, roadmap
1A-vision/               # Product Owner — problem, goals, personas
2A-research/             # Business Analyst — ecosystem, competitors
3A-requirements/         # PO + BA — scope, MVP definition
4A-design/               # UX Designer — interaction model, wireframes
4B-architecture/         # Developer — system design, tech stack
5A-specs/                # Developer — feature specifications
5B-quality/              # QA — test strategy, edge cases
6A-build-plan/           # Developer + PO — implementation plan
7A-app/                  # Developer — source code
AA-journal/              # Session logs, changelog
AB-decisions/            # Decision records, ADRs
CLAUDE.md                # Root governance — session protocol
```

Every folder has governance rules. Every role has clear boundaries. Every session starts with context, not confusion.

---

## Quick Start

1. **Copy** [`bootstrapping-guide.md`](bootstrapping-guide.md) into your conversation with any AI agent
2. **Tell the agent** what you're building (one paragraph is enough)
3. **Answer its questions** about your project (team size, UI, public release, etc.)
4. **Get your structure** — the agent scaffolds everything and generates a discussion roadmap

That's it. Your next session, the agent reads the dashboard, picks up where you left off, and knows exactly what role it's playing.

---

## Core Concepts

### Hats (Roles)

Every perspective gets a named role — **Product Owner**, **Developer**, **QA**, **User**, etc. The AI declares which hat it's wearing before doing any work. This prevents the common failure mode where AI agents blur concerns (debating architecture when you asked about user needs).

### Tiers (Influence Radius)

Folders are ordered by how far their decisions ripple. Vision (tier 1) shapes everything downstream. App code (tier 7) affects only itself. This ordering determines what to discuss first and what depends on what.

### Three-Layer Governance

```
Root CLAUDE.md          →  Project structure, conventions, session protocol
  └─ 0A-ceo/hats.md    →  Role definitions and boundaries
    └─ Per-folder CLAUDE.md  →  What belongs here, what doesn't, who reads this
```

The AI reads all three layers at session start. No more "I forgot what we decided last time."

### Discussion Roadmap

The scaffold is empty on purpose. The roadmap is a phased plan for filling it — vision first, then personas, then research, then scope, then architecture, then specs, then code. Each phase has clear deliverables and dependencies.

---

## Project Complexity

The guide scales to your needs:

| Complexity | Team | Hats | Example |
|-----------|------|------|---------|
| **Minimal** | Solo, simple tool | 4-5 | Open source library, CLI utility |
| **Standard** | Solo or small team, product with UI | 5-8 | SaaS app, mobile app, dev tool |
| **Complex** | Team, platform, compliance | 8-12 | Multi-service platform, regulated product |

---

## Works With Any AI Agent

The guide is written for AI agents generically. It works with:

- **Claude** (Claude Code, claude.ai, API)
- **ChatGPT / GPT-4**
- **Gemini**
- **GitHub Copilot Chat**
- **Cursor, Windsurf, or any AI-assisted IDE**

The governance files use `CLAUDE.md` as the filename convention (since Claude Code reads these natively), but the system works regardless of which AI you use — the agent reads the files because the protocol tells it to, not because of any tool-specific integration.

---

## FAQ

**Do I need to know what "hats" are before starting?**
No. The guide teaches the AI agent the system. The agent then walks you through choosing the right roles for your project.

**Can one person wear multiple hats?**
Yes. The folder structure doesn't change based on team size. A solo developer wears all the hats — the structure just keeps the perspectives separate.

**Is this only for new projects?**
It's designed for bootstrapping new projects, but you could adapt the governance layer to an existing codebase by creating the folder structure alongside your existing code.

**What if I don't need all those folders?**
The agent will help you choose. A minimal project might only have 7 folders. The guide explicitly warns against over-scaffolding.

**Does this replace project management tools?**
No. This is a governance and thinking framework, not a task tracker. It complements tools like Linear, Jira, or GitHub Issues.

---

## Examples in the Guide

The guide includes complete examples for:
- **Minimal** — Open source library (4 hats, 7 folders)
- **Standard** — Solo CLI tool (5 hats, 10 folders)
- **Complex** — Team SaaS platform (10 hats, 16 folders)
- **Data-heavy** — Analytics product (6 hats, 10 folders)

Plus full templates for every governance file: root CLAUDE.md, hat profiles, folder CLAUDE.md, status dashboard, discussion roadmap, decision records, session logs, and more.

---

## Contributing

Found a gap in the guide? Have a project type that doesn't fit the patterns?

1. Open an issue describing the scenario
2. If you have a fix, submit a PR against `bootstrapping-guide.md`

Keep contributions focused on the guide itself — this repo is intentionally a single file with a README.

---

## Support This Project

If this guide saved you hours of project chaos, consider supporting its development:

<a href="https://buymeacoffee.com/ThanhWilliamLe">
  <img src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black" />
</a>

<!-- Uncomment and configure the sponsorship options that apply to you:

[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?style=for-the-badge&logo=github-sponsors)](https://github.com/sponsors/ThanhWilliamLe)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-F16061?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/ThanhWilliamLe)
-->

---

## Star History

If you find this useful, a star helps others discover it.

[![Star History Chart](https://api.star-history.com/svg?repos=ThanhWilliamLe/ai-product-bootstrap&type=Date)](https://star-history.com/#ThanhWilliamLe/ai-product-bootstrap&Date)

---

## License

[MIT](LICENSE) — use it however you want.
