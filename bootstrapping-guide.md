# Bootstrapping a Product Development Project with AI Agents

> This guide is for AI agents. A user gives you this file and asks you to bootstrap their project. Follow the protocol below.

> This guide is standalone. It is not specific to any product, technology, or AI tool. It works with any agent that reads markdown — Claude, ChatGPT, Gemini, Copilot, Cursor, or others. The convention of naming files `CLAUDE.md` is a project-governance pattern, not a Claude-specific feature.

---

## Table of Contents

1. [How to Use This Guide](#1-how-to-use-this-guide)
2. [Bootstrapping Protocol](#2-bootstrapping-protocol)
3. [Reference: Core Concepts](#3-reference-core-concepts)
4. [Reference: Naming Conventions](#4-reference-naming-conventions)
5. [Reference: Cross-Hat Rules](#5-reference-cross-hat-rules)
6. [Reference: Session Protocol](#6-reference-session-protocol)
7. [Examples](#7-examples)
8. [Anti-Patterns to Avoid During Bootstrap](#8-anti-patterns-to-avoid-during-bootstrap)
9. [Post-MVP Lifecycle](#9-post-mvp-lifecycle)
10. [Templates](#10-templates)

---

## 1. How to Use This Guide

### Who reads this

An AI agent, given this file by a user who wants to bootstrap a new product development project.

### What you'll produce

By the end of the protocol, the user will have a project structure that:
- **Organizes work by role** — folders for each perspective (product thinking, design, architecture, code, testing), so nothing gets mixed up
- **Plans what to discuss and when** — a phased roadmap of conversations that turns "I have an idea" into a defined product, session by session
- **Lets agents pick up where they left off** — a dashboard with machine-readable status so the next session starts without re-explaining context
- **Prevents mistakes** — each folder has rules about what belongs there, what doesn't, and where to put misplaced work
- **Records decisions** — a journal and decision log so nothing is lost between sessions

Concretely, this means: a folder structure with role-based workspaces, a CLAUDE.md file in each folder with ownership rules, a CEO dashboard tracking progress, a discussion roadmap for phased discovery, and session logs. Everything in plain markdown, committed to git.

### What you know and don't know

| You know (from this guide) | You don't know (need from user) |
|---|---|
| The system, patterns, templates | What the product is |
| How to choose hats and assign tiers | Which hats are relevant |
| What governance files look like | What deliverables each hat needs |
| How to generate a discussion roadmap | The user's team size, constraints, preferences |

### What the user knows and doesn't know

| User typically knows | User typically doesn't know |
|---|---|
| Their product idea (maybe vague) | What hats are or how to choose them |
| Who's involved (solo? team?) | What tiers or influence radius means |
| That they want structure | How to write governance files |
| Rough sense of what's needed | The specific folder/file taxonomy |

Your job: ask the right questions, make decisions together, then scaffold everything.

### If you want to see what you're building first

Jump to Section 7 (Examples) to see concrete folder structures for different project sizes — from a 7-folder weekend library to a 17-folder team SaaS product. Then come back here to build yours.

---

## 2. Bootstrapping Protocol

### Mental Model

Three concepts, clearly distinct:

- **Hats** are roles/perspectives (Product Owner, Developer, QA). They define how you think.
- **Folders** are workspaces where hats produce artifacts. One hat can own multiple folders (Developer owns architecture, specs, build-plan, and app). One folder can have multiple contributing hats (Requirements is co-owned by PO and BA).
- **Tiers** order folders by influence radius — how far a folder's decisions ripple. Tiers are assigned to folders, not to hats. A hat that owns folders at multiple tiers (like Developer) simply works across tiers.

The bootstrapping flow: choose hats → map hats to folders → assign tiers to folders → scaffold → govern.

### Step 0: Assess Project Complexity

Before starting, classify the project:

| Complexity | Indicators | What to scaffold |
|---|---|---|
| **Minimal** (solo, simple tool, library) | 1 person, no UI, no market research, no public release | 4-5 hats, skip plans subfolders, skip priorities.md, minimal discussion roadmap (~5 phases). Cross-cutting folders can be simplified: `AB-decisions/` can be a single `decisions.md` file instead of a folder. Journal is optional. |
| **Standard** (solo/small team, product with UI) | 1-3 people, has UI, may be public, moderate complexity | 5-8 hats, full governance layer, ~10-phase discussion roadmap. This is the guide's default. |
| **Complex** (team, platform, compliance) | 4+ people, multiple services, regulatory requirements, public release | 8-12 hats (the "8 max" guidance is a soft cap — complex projects legitimately need more), extensive discussion roadmap with many custom phases, consider RACI or team assignment docs beyond what this guide covers. |

For **minimal** projects: Steps 3 (tier assignment) and 7 (CEO dashboard) can be lightweight. The tier ordering is usually self-evident. The dashboard can be a few lines, not a full table.

For **complex** projects: The guide covers structure and governance but NOT team coordination (who wears which hat, approval workflows, concurrent editing). You may need supplementary team processes beyond this guide.

Follow the steps below. Each step lists what to ask, what to decide, and what to generate.

### Step 1: Understand the Product

**Ask the user:**
- What are you building? (one paragraph is enough)
- Who is it for?
- Solo or team?
- Is there a user interface? (GUI, CLI, API, none)
- Will this be publicly released?
- Any existing work to incorporate?

**Don't ask yet:** technical stack, architecture details, feature lists. Those come later during the discussion phases, not during bootstrapping. If the user volunteers this info unprompted, acknowledge it and note it for the architecture phase — don't ignore it, but don't let it drive bootstrapping decisions.

#### Heavy Assets Decision Gate

After the general questions above, explicitly raise this topic — don't let it slip by:

> "Will this project involve large or binary files that shouldn't live in git? For example: videos, screen recordings, hi-res design exports, Figma/PSD source files, demo recordings, pitch deck videos, user interview recordings."

**Why this matters early:** If heavy assets are part of the project, the infrastructure (`heavy-assets/` folder, `project-tools/` with sync tooling, `.gitignore` rules, cloud storage choice) must be scaffolded during bootstrap — not bolted on later. Retrofitting a heavy-assets workflow after work has started means files end up committed to git by accident, scattered in random locations, or lost between machines.

**If the user says yes (or "probably" / "maybe"):**
1. Note it — this triggers the Infrastructure Folders scaffolding in Step 5.
2. Ask: "Where would you sync these to? Google Drive, Dropbox, S3, or something else?" (This informs the sync spec template.)
3. Ask: "Which hats will produce heavy files?" (This determines which subdirectories to pre-create inside `heavy-assets/`.)

**If the user says no:** Skip infrastructure folders entirely. They can be added later if needs change.

### Step 2: Choose Hats

**Present the common hats table and help the user pick:**

| Hat | What it owns | Include when... |
|-----|-------------|-----------------|
| **CEO** | Cross-hat coordination, status, priorities | Always |
| **Product Owner** | Vision, goals, scope, prioritization | Always |
| **Business Analyst** | Research, data, competitive analysis | Market research or ecosystem analysis needed |
| **UX Designer** | Interaction design, wireframes, user flows | Product has a user interface |
| **Developer** | Architecture, specs, code, build planning | Always |
| **QA** | Test strategy, edge cases, quality gates | Always (formality varies) |
| **User** | Validation, dogfooding, honest reactions | Always |
| **Marketing/DevRel** | Positioning, copy, community, launch | Public release planned |
| **Repo Ops** | Release management, publish workflow, community triage | Public repo, GitHub/GitLab releases, or separate publish repo |
| **Data Engineer** | Data pipelines, schemas, ETL | Data-heavy product |
| **Security** | Threat modeling, auth, compliance | Sensitive data or attack surface |
| **DevOps/Platform** | CI/CD, infrastructure, deployment | Complex deployment needs |

**Decision logic:**
- Start with 4 mandatory: CEO, Product Owner, Developer, User.
- Add QA if quality matters (almost always yes).
- Add UX Designer if there's a user interface.
- Add BA if there's an ecosystem to research or a market to analyze.
- Add Marketing if the product will be public.
- Add Repo Ops if the product has a public repo, GitHub releases, or a separate publish repo. Often added post-MVP when launch approaches — see Post-MVP Lifecycle.
- Add others based on product specifics.
- Aim for 4-8 hats. Fewer than 4 = probably merging perspectives that should be separate. More than 8 = probably splitting too fine (soft cap — complex projects may legitimately need 8-12; see Step 0).

**Important:** One person can wear multiple hats. The folder structure doesn't change based on team size — only the hat declarations change.

**Special hats:**
- **User** has no folder. It validates other hats' work.
- **CEO** stays lightweight — dashboards and pointers, not heavy content.

### Step 3: Assign Tiers

**For each folder, ask:** "If the decisions in this folder change, how many other folders are affected?"

- Most affected by changes → lowest tier number (broadest radius)
- Least affected by changes → highest tier number (narrowest radius)
- Same impact → same tier (peers, use letter suffixes: 4A, 4B)

Note: a single hat (like Developer) may own folders at different tiers — that's expected. Tiers order folders, not hats.

**Default tier pattern** (adjust based on the specific project):

```
Tier 0: CEO
Tier 1: Vision, Roadmap
Tier 2: Research
Tier 3: Requirements
Tier 4: Design, Architecture (peers — they constrain each other)
Tier 5: Specs, Quality (peers — contracts and validation)
Tier 6: Marketing, Rollout, Build Plan, Release (downstream peers)
Tier 7: App code (narrowest)
```

**Discuss with the user:** Walk through the tier assignments and ask if they feel right. The user may have intuitions about which concerns are more foundational in their specific product.

**About peers:** Same-tier means "co-dependent" (both constrain each other) or "independent" (no dependency). If one clearly feeds the other with no feedback loop, they should be at different tiers.

### Step 4: Define Deliverables per Hat

**For each hat folder, determine:**
- What stub `.md` files to create (the planned deliverables)
- What subfolders are needed (only if the hat produces multiple artifact types)
- Whether a `plans/` subfolder is needed (optional — only if work is non-obvious or multi-session)

**Ask the user about each hat:** "What documents or artifacts will the [hat name] hat produce?" Guide them with examples from this table:

| Hat / Folder | Typical deliverables | Typical subfolders |
|-----|---------------------|-------------------|
| CEO | status-dashboard.md, priorities.md, cross-hat-dependencies.md, hats.md, discussion-roadmap.md | — |
| Product Owner | problem-statement.md, product-goals.md, personas.md | — |
| Product Owner (Roadmap) | roadmap.md, release-planning.md | — |
| Business Analyst | ecosystem-research.md, competitive-landscape.md, format-mapping.md, research-notes.md (methodology: source verification, cross-referencing official docs) | comparisons/, diagrams/, data/, plans/ |
| UX Designer | interaction-model.md (include visual identity: palette, typography, component colors) | wireframes/, mockups/, prototypes/ (HTML prototypes encouraged — interactive, self-contained), assets/, plans/ |
| Developer (Architecture) | data-model.md, tech-stack.md, system-design.md | diagrams/ |
| Developer (Specs) | (populated just-in-time — one spec per feature) | — |
| Developer (Build Plan) | implementation-plan.md, milestones.md (each milestone: scope, dependencies, quality gate, done criteria) | — |
| Developer (App) | source code, CI/CD config (.github/workflows/ or equivalent). CI at minimum: lint, typecheck, test, build. Multi-platform matrix if desktop/native. | — |
| QA | test-strategy.md (comprehensive: unit, integration, API/contract, component, E2E, security, performance — see Phase 8 for full list; per-milestone quality gates), edge-cases.md | reports/, plans/ |
| Marketing | messaging-guide.md | copy/, collateral/, screenshots/, plans/ |
| Rollout | distribution-strategy.md, first-run-experience.md | — |
| Repo Ops | publish-checklist.md, release-best-practices.md | — |
| Data Engineer | data-model.md, pipeline-specs.md, data-quality-rules.md | schemas/, pipelines/, data-quality/ |
| Security | threat-model.md, auth-design.md, compliance-checklist.md | — |
| DevOps/Platform | deployment-strategy.md, infrastructure-spec.md, ci-cd-pipeline.md | — |

**Don't over-scaffold.** If a folder will only have 2-3 files, it doesn't need subfolders. If a deliverable is uncertain, don't create a stub — it can be added later. However, if a folder CLAUDE.md's "Read before producing work" will reference a file, that file must exist as a stub at minimum.

**Determine folder ownership:** Most folders are owned by one hat. Some folders have multiple contributing hats. Decide co-ownership when:
- Two hats produce different types of artifacts in the same folder (e.g., Requirements: PO defines scope, BA analyzes feasibility)
- The folder's content requires both a technical and non-technical perspective (e.g., Specs: Developer writes schemas, BA validates against requirements)

For co-owned folders, document the split in the folder's CLAUDE.md: what each hat does there, and for change requests, which type of change each hat can make (see Section 5 → "Co-Owned Folders").

**Determine multi-hat plan folders:** If a plan serves multiple hats (product roadmap, build plan, rollout plan), it gets its own top-level folder at the appropriate tier. Single-hat plans go in `plans/` subfolders.

Examples of multi-hat plans:
- **Product roadmap** (PO hat, but consumed by all) → top-level at tier 1
- **Build plan** (Developer hat, but coordinates with PO and QA) → top-level at tier 6
- **Research plan** (BA hat only) → `plans/` subfolder inside research folder

### Step 5: Scaffold Folders

**Generate the folder structure:**

```
0A-ceo/
  status-dashboard.md
  priorities.md
  cross-hat-dependencies.md
  hats.md
  discussion-roadmap.md
{tier}{letter}-{name}/
  {subfolders}/
  CLAUDE.md
AA-journal/
  sessions/
  changelog.md
AB-decisions/
  session-decisions.md
  adr/
```

**Letter assignment rules:**
- Always start with `A` even if there's only one folder at a tier: `3A-requirements/` not `3-requirements/`.
- Cross-cutting folders always use double letters starting from `AA`: `AA-journal/`, `AB-decisions/`.
- These prefixes are conventions, not project-specific choices — use them consistently.

**Actions:**
1. Create all hat folders with tier prefixes.
2. Create `0A-ceo/` with its files:
   - Always: `status-dashboard.md`, `hats.md`, `discussion-roadmap.md`
   - Standard/Complex only: `priorities.md`, `cross-hat-dependencies.md`
   - For minimal projects (see Step 0), skip priorities and cross-hat-dependencies — they add overhead without value for solo work.
3. Create cross-cutting folders (`AA-journal/`, `AB-decisions/`).
4. Create infrastructure folders if needed — no tier prefix. See **Infrastructure Folders** below.
5. Create all subfolders within hat folders.
6. Add `.gitkeep` to empty subfolders (remove once real files are added).
7. Create all stub files with title + one-line description:
   ```markdown
   # {Title}

   > {One-line description of what this file will contain.}
   ```
8. Set up `.gitignore`:
   ```
   # OS
   .DS_Store
   Thumbs.db
   Desktop.ini

   # IDE
   .vscode/
   .idea/
   *.swp
   *.swo
   ```
9. If the project will have source code, scaffold CI/CD early:
   - Create `.github/workflows/ci.yml` (or equivalent for the user's platform)
   - Minimum pipeline: checkout → setup runtime → install deps → lint → typecheck → test → build
   - For desktop/native apps: add platform matrix (ubuntu, windows, macos)
   - Working directory should target the app folder
   - CI should run on push to main and on pull requests

#### Infrastructure Folders (Optional — triggered by Step 1 Heavy Assets Decision Gate)

**Skip this section** if the user said "no" to heavy assets in Step 1. Otherwise, scaffold both folders now.

```
heavy-assets/          # Gitignored — large/binary files, mirroring hat structure
  CLAUDE.md            # Always committed — explains the folder
  {hat-folder}/        # Only for hats identified in Step 1 as producing heavy files
    {subfolders}/      # e.g., 4A-design/mockups/, 6A-marketing/demo-videos/
project-tools/         # Committed — scripts that operate on the project, not the product
  CLAUDE.md
  sync-heavy-assets.sh
  sync-heavy-assets.spec.md
```

Pre-create only the subdirectories for hats the user identified in Step 1 as producing heavy files. Don't speculatively add subdirectories for every hat.

**What goes where:**

| Folder | Committed to git? | Contains |
|--------|-------------------|----------|
| `heavy-assets/` | No (gitignored, except `CLAUDE.md`) | Large/binary files organized by hat |
| `project-tools/` | Yes | Sync scripts, setup scripts, workflow helpers |
| `.heavy-assets.secret` | No (gitignored) | Machine-local sync credentials |

**Scaffolding actions:**
1. Create `heavy-assets/` with a `CLAUDE.md` (use the Heavy Assets CLAUDE.md template in Section 10).
2. Pre-create subdirectories mirroring hat folders that will produce large files (e.g., `heavy-assets/4A-design/mockups/`). Add `.gitkeep` to empty ones.
3. Create `project-tools/` with a `CLAUDE.md` (use the Project Tools CLAUDE.md template in Section 10).
4. Create `project-tools/sync-heavy-assets.spec.md` — generate a full spec covering the topics listed in Section 10's Sync Heavy Assets Spec guidance. Adapt the cloud provider to match the user's answer from the Step 1 decision gate.
5. Add these entries to `.gitignore`:
   ```
   # Heavy assets
   heavy-assets/*
   !heavy-assets/CLAUDE.md

   # Sync tool secrets
   .heavy-assets.secret
   ```

#### Publish Repo Pattern (Optional — for public releases)

**Skip this section** if the product won't be publicly released or open-sourced. Otherwise, plan the dual-repo pattern now.

**The pattern:** Maintain two repositories:
- **Governance repo** (this project) — contains all hat folders, governance files, session journals, decision records, AND the app source code.
- **Publish repo** (sibling folder) — receives a curated subset of files for public consumption: source code, README, CONTRIBUTING.md, LICENSE. No governance files, no design docs, no session journals.

**Why separate:**
- Internal design decisions, personas, and competitive research don't belong in a public repo.
- The governance repo contains the methodology; the publish repo contains the product.
- Keeps the public repo clean and focused on what users need.

**Scaffolding actions:**
1. Create the publish repo as a sibling folder (e.g., `../{project-name}-app-{product-name}/`).
2. Add a Release/Publish folder in the governance repo (e.g., `6D-release/`) with:
   - `publish-checklist.md` — step-by-step publish procedure
   - `release-best-practices.md` — asset naming, checksums, changelog format
   - `CLAUDE.md` — governance for publish workflow
3. Add `.env` to `.gitignore` — store publish repo PAT and URL there.
4. The Repo Ops hat owns the publish workflow: sync source → copy marketing README → commit → push → create GitHub release.

**What gets synced to the publish repo:**
- `{app folder}/` source code (excluding `node_modules/`, `dist/`, `coverage/`, `CLAUDE.md`)
- Marketing README from `{marketing folder}/readme-draft.md` → publish repo `README.md`
- `CONTRIBUTING.md` from marketing folder
- `LICENSE` from root

**What stays in the governance repo only:**
- All numbered hat folders (0A through 7A+)
- Cross-cutting folders (AA, AB)
- Heavy assets
- `.env` files
- Per-folder CLAUDE.md files

**Marketing deliverables workflow:** When Marketing hat is active and preparing for launch:

1. **Messaging guide** — one-liner, elevator pitch, value props, tone rules, words-to-avoid. Test with personas.
2. **README for users** — not developers. Cut architecture, design, and internal sections. Keep: what it does, screenshot/GIF, quick start, features. Test with 3-5 personas — converge on what to cut.
3. **Landing page** (if applicable) — HTML mockup with design tokens from the UX phase.
4. **Announcement posts** — platform-adapted (Reddit, HN, Discord). Different tone per platform.
5. **Comparison guide** — honest positioning vs competitors. Keep separate from README.
6. **Demo GIF/video** — 4-6 frame walkthrough with captions.
7. **Slop audit** — verify zero AI cliché words across all deliverables ("revolutionary", "seamlessly", "cutting-edge", "empower").

Research what established projects in your space do for their READMEs. Study 3-5 mature repos for patterns.

### Step 6: Generate Governance Files

Generate these files in order — each one builds on the previous:

#### 6a: Hat Profiles (`0A-ceo/hats.md`)

One file, all hats. Each profile follows this structure:

```markdown
## {Hat Name}

> {One-line role summary}

- **Mindset:** {How to think — an orientation, not a task list}
- **Cares about:** {What this hat focuses on}
- **Does NOT:** {What this hat leaves to others — name which hat handles it}
- **Quality bar:** {Testable criteria for "done well"}
- **Primary folders:** {Which folders this hat works in}
```

**Writing tips:**
- Mindset should be a thinking orientation: "Evidence-based. Claims need sources." not "Research ecosystems."
- "Does NOT" is critical — it prevents scope creep between hats.
- Quality bar must be testable: "Every requirement has a test path" not "High quality."

#### 6b: Per-Folder CLAUDE.md

Create a CLAUDE.md in every folder (hat folders + cross-cutting + infrastructure). Each follows this structure:

```markdown
# {tier}{letter}-{folder-name} — {Short Description}

## Contributing hats

### As {Hat 1}
- {What this hat does in this folder}
- {Specific guidance}

### As {Hat 2} (if multi-hat folder)
- {What this hat does in this folder}

## What belongs here
- {Artifact type — brief description}

## What does NOT belong here
- {Misplaced type} → `{correct-folder}/`

## Subfolders (if any)
- `{name}/` — {what it contains}

## Read before producing work
- `{path}` — {why}

## Downstream consumers
- `{folder}/` — {how they use this output}
```

**Critical:** The "What does NOT belong here" section with redirects is the most operationally valuable part. Think about what an agent might reasonably get wrong for each folder and add explicit redirects.

**"Read before producing work"** should list only direct upstream dependencies — what must be read before producing work in this folder. Follow the tier ordering: higher-tier folders are upstream of lower-tier folders.

**"Downstream consumers"** should list who uses this folder's output. This creates accountability and helps agents understand ripple effects.

#### 6c: Root CLAUDE.md

The project entry point. Use the template in Section 10.

Includes: session start protocol, structure listing, conventions, three-layer governance description, cross-hat rules, hat-to-folder table, start-here links, plus protocol sections (multi-round auditing, docs-over-memory, autonomous continuation, user testing, end-of-session sync, output location).

**Do not put in root CLAUDE.md:** current status (goes in CEO dashboard), detailed hat profiles (goes in hats.md), technology decisions (goes in architecture folder), anything that changes frequently.

### Step 7: Initialize CEO Dashboard

Populate `0A-ceo/status-dashboard.md` with the initial project state:
- Current phase: "Project bootstrapped. Ready for Phase 1 of discussion roadmap."
- Current milestone: "None — milestones defined during Build Plan Consolidation phase."
- Next action: CEO hat, "Begin Phase 1 of discussion roadmap", target `{vision folder}/`.
- All hats listed with status "Not Started" (except CEO: "Active").
- No blockers (unless the user mentioned constraints).
- Recently completed: "Project structure scaffolded."
- Validation status: "Not started."

The Next Action section is what enables autonomous continuation — an agent reading this dashboard knows exactly what to do next without asking.

### Step 8: Generate Discussion Roadmap

**This is critical.** The folder structure is empty scaffolding. The discussion roadmap is the plan for how to fill it through phased discussions across future sessions.

#### What a discussion roadmap is

A sequence of focused discussion phases, each with:
- A topic to discuss
- Which hat(s) are active
- A goal for the discussion
- Which stub files get populated as deliverables
- Dependencies on prior phases

#### Default phase ordering

Phases follow the tier ordering naturally — broader concerns first, narrower later:

```
Phase 1: Vision & Problem Statement
  Hats: Product Owner
  Goal: Define what problem we solve and why it matters
  Populates: {vision folder} — problem-statement.md, product-goals.md

Phase 2: User Personas & Use Cases
  Hats: Product Owner, UX Designer (if present), User
  Goal: Identify who uses this and what workflows they need
  Populates: {vision folder} — personas.md; {requirements folder} — use-cases.md

Phase 3: Research & Ecosystem (if BA hat exists)
  Hats: Business Analyst
  Goal: Investigate the ecosystem, competitive landscape, feasibility
  Populates: {research folder} — all research files
  Methodology: Verify claims against official docs/repos. Cross-reference multiple
    sources. Flag assumptions vs verified facts. Document methodology in research-notes.md.
  Note: Can run in parallel with Phase 2
  BLOCKING: If research reveals the ecosystem is different than assumed,
  Phase 4+ cannot start until research is verified. Mark as hard blocker.

Phase 4: Feature Scope & MVP
  Hats: Product Owner, Business Analyst (if present)
  Goal: Define MVP boundary — what ships, what doesn't
  Populates: {requirements folder} — feature-scope.md, mvp-definition.md
  Depends on: Phases 1, 2, (3 if BA exists — hard blocker if research is foundational)
  Deferral stress-test: For each deferred feature, ask: "Would the scaled persona
    (power user at 2-3x normal load) find this painful to lack?" If yes, the
    deferral may be wrong. Common false deferrals: identity resolution helpers,
    data visualization (tables aren't enough), third-party integration depth
    (regex links aren't enough — users want enrichment).

Phase 5: UX Flows & Interaction Design (if UX Designer hat exists)
  Hats: UX Designer, User
  Goal: Define how users interact with the product
  Populates: {design folder} — interaction-model.md, wireframes/
  Depends on: Phases 2, 4
  Design variant exploration: Build 2-4 visual variants (e.g., different aesthetics,
    color palettes, layout approaches). Evaluate with User hat + other hats for
    perspective diversity. Choose and document the aesthetic direction in
    interaction-model.md. Auditing the prototype against all upstream specs catches
    design drift early.

Phase 6: Architecture & Tech Stack
  Hats: Developer
  Goal: Choose technology, design the system
  Populates: {architecture folder} — tech-stack.md, system-design.md, data-model.md
  Depends on: Phases 4, (5 if UX exists)

Phase 7: Detailed Design (varies by project)
  Hats: Developer, UX Designer, others as needed
  Goal: Deep-dive on specific subsystems (e.g., portability, plugin discovery)
  Populates: Additional files in architecture, design, specs folders
  Depends on: Phase 6

Phase 8: Testing & Quality Strategy
  Hats: QA
  Goal: Define comprehensive test strategy across all product layers
  Populates: {quality folder} — test-strategy.md, edge-cases.md
  Depends on: Phases 4, 6
  Details:
    test-strategy.md must address ALL applicable categories below.
    For each included type: specify framework, coverage target, when it runs
    (CI vs local vs manual), which milestones introduce it.
    Skip types with explicit rationale ("No load tests: single-user local app").

    FUNCTIONAL TESTING (does it work?):
    - Unit — isolated functions, pure logic, parsing, validation, transforms
    - Integration — modules together, real/in-memory DB, service interactions, fixtures
    - API / contract — HTTP routes, response shapes, status codes, error formats, auth
    - Component — UI renders correctly, props/state, interactions, a11y attributes
    - System — full app in production-like environment, all services connected
    - E2E — critical user journeys start-to-finish (one per core use case minimum,
      derive from use-cases.md, list each journey explicitly)
    - Smoke — quick post-build check that major functions work (run in CI on every push)
    - Sanity — narrow check after a targeted fix, confirm fix works + no side effects
    - Regression — re-run existing tests after changes to catch breakage (CI handles
      this automatically if test suite is comprehensive; note any manual regression checks)

    NON-FUNCTIONAL TESTING (does it work well?):
    - Performance — response time baselines, render benchmarks, memory profiling
      - Load — behavior under expected concurrent usage
      - Stress — behavior beyond capacity, graceful degradation
      - Endurance / soak — sustained load over time, memory leak detection.
        Implementation: write tests that exercise core subsystems through 200-1000
        iteration cycles each (CRUD churn, state store toggling, serialization
        round-trips, filter cycling). Verify no unbounded array growth, no stale
        Map entries, no file handle leaks. Run as part of test suite, not separate
        infrastructure. Example: 57 endurance tests across main process and renderer
        stores caught accumulation bugs that unit tests missed.
    - Security — input sanitization, auth bypass, path traversal, XSS, CSRF, secrets
      exposure, dependency vulnerability scanning (if Security hat exists, co-own)
    - Usability — four tiers of user-experience validation, each catching different
      bug classes. Run in order; each tier is slower but catches issues invisible
      to the previous tier:
      1. UAT (code-level) — single QA agent walks predetermined scenarios against
         source code, scores PASS/FAIL. Catches dead features, wiring bugs, broken
         promises, swallowed errors. Fast (~30 min). See Post-Build validation loop.
      2. Simulated user testing — 3-5 AI persona sub-agents read source code and
         imagine using the app from their persona's perspective. Catches jargon,
         wrong mental models, terminology inconsistency, first-run confusion,
         missing tooltips. Fast (~5 min, parallel agents). Run before live testing.
      3. Live agent testing — AI persona sub-agents build and launch the real app,
         then interact via Playwright MCP tools (screenshot, click, fill, navigate).
         They see real rendered pixels, not source code. Catches visual overlap,
         broken click targets, timing/animation bugs, state desync, scroll/overflow
         issues, visual hierarchy problems — an entire class of bugs invisible to
         code review. Medium speed (~20-30 min, sequential agents sharing one app
         instance). Run after simulated testing fixes land.
      4. Real human testing — 3-5 actual humans use the app unguided. Catches
         emotional reactions, motor skill issues, cultural assumptions, real-world
         workflow interruptions. Slow (1-2 hrs per participant). Run after live
         agent testing passes.
      Recommended order: simulated → fix → live agent → fix → human.
      For minimal projects, tier 1 (UAT) alone may suffice. For any product with
      a UI, tiers 1-3 are recommended before release. Tier 4 before public launch.
    - Accessibility — WCAG compliance, screen reader, keyboard nav, color contrast
    - Compatibility — target OS/browser/device matrix, responsive breakpoints
    - Snapshot / visual regression — UI consistency across changes (if UI exists)

    EXECUTION NOTES:
    - Automated (CI): unit, integration, API, component, E2E, smoke, regression, snapshot
    - Sub-agent (pre-release): UAT validation loop, simulated user testing, live agent testing
    - Manual: real human usability, accessibility audit, exploratory
    - The validation loop (Post-Build phase) covers UAT (tier 1). Simulated and live
      agent testing (tiers 2-3) run alongside or after the validation loop — see
      Section 6 for the full protocol.
    - Regression is implicit when CI runs full suite; note any MANUAL regression checks

Phase 9: Rollout & Distribution (if applicable)
  Hats: Product Owner, Developer
  Goal: How users get the product, update mechanism, first-run UX
  Populates: {rollout folder} — distribution-strategy.md, first-run-experience.md
  Depends on: Phase 6

Phase 10: Marketing & Launch (if Marketing hat exists)
  Hats: Marketing/DevRel
  Goal: Positioning, copy, go-to-market
  Populates: {marketing folder}
  Depends on: Phases 1, 4, (5 if UX exists)

Final: Build Plan Consolidation
  Hats: Developer, Product Owner, QA
  Goal: Design milestones with quality gates, define done criteria per milestone
  Populates: {build-plan folder} — implementation-plan.md, milestones.md
  Depends on: All above phases
  Details:
    - Each milestone: scope (what ships), dependencies (what must exist first),
      quality gate (automated commands: lint, typecheck, test, build),
      done criteria (specific tests passing, coverage target)
    - Milestone state machine: not-started → in-progress → gates-passing → complete
    - QA hat defines per-milestone test requirements (which layers, what coverage)
    - First milestone is always scaffold (toolchain, CI, empty shell)
    - Last milestone is always integration & polish (full suite green)

Post-Build: Automated Validation Loop
  Hats: CEO (orchestrator), User (validator), all other hats (fixers)
  Goal: Sub-agent validation until 97% scenario success rate
  Populates: {quality folder}/reports/, {journal folder}/sessions/
  Depends on: All milestones complete
  Details:
    - CEO spawns sub-agents wearing each persona hat to test real user scenarios
    - Each sub-agent: attempts use cases end-to-end, reports pass/fail with evidence
    - Failures filed as issues → owning hat fixes → re-test cycle
    - Loop continues until success rate ≥ 97% across all scenarios
    - See Section 6 → "Automated Validation Loop" for the full protocol
```

#### How to customize the roadmap

- **Skip phases** for hats that don't exist (no BA → skip Phase 3).
- **Merge phases** that are small enough to cover in one session.
- **Split phases** that are too large (Phase 7 often becomes multiple phases for complex products). When splitting, use decimal numbering (Phase 7.1, 7.2) or insertion numbering (Phase 7, Phase 7.5, Phase 8).
- **Add phases** for project-specific concerns (security review, data architecture, compliance).
- **Reorder** if the project has unusual dependency structure.
- **Use Phase N.5 for revisiting earlier work.** A common pattern: do light research early (Phase 1.5), then deep research later (Phase 3.5) that may backtrack earlier assumptions. This prevents over-investing in early phases while still catching assumption errors. The light phase gives "good enough" context to proceed; the deep phase verifies and corrects before downstream phases lock in. Real example: Phase 3.5 deep ecosystem research corrected major assumptions from Phases 1-3 (e.g., "Antigravity is an IDE, not a CLI" — this changed the entire adapter strategy).

#### Blocking prerequisites

Some phases are **hard blockers** — later phases cannot start until these complete, because the later work would be built on unverified assumptions. Mark these explicitly in the roadmap.

Common blocking patterns:
- Research blocks architecture — if the ecosystem is poorly understood, don't design the system yet.
- Architecture blocks implementation — don't code without a system design.
- MVP definition blocks everything downstream — scope must be locked before designing, building, or testing.

Use this format in the roadmap:
```
- **Depends on:** Phase 3 (BLOCKING — architecture decisions depend on verified research)
```

vs. soft dependencies:
```
- **Depends on:** Phase 5 (informational — UX flows improve architecture but aren't required)
```

#### Generate the file

Create `0A-ceo/discussion-roadmap.md` with a Folder Map (which hat owns which folder), then the phases and progress tracker. Use the template in Section 10:

```markdown
# Discussion Roadmap

> Established: {date}
> Status: Not Started — Phase 1

## Phases

### Phase 1 — {Title}
- **Hats:** {active hats}
- **Goal:** {one line}
- **Deliverables:**
  - `{path/to/file.md}`
- **Depends on:** {prior phases or "None"}

{...repeat for all phases...}

## Progress Tracker

| Phase | Status | Date Started | Date Completed |
|-------|--------|--------------|----------------|
| 1. {Title} | Not Started | | |
| ... | | | |
```

**Important:** The discussion roadmap is a living document. Phases may be reordered, split, merged, or added as the project evolves. It is NOT a rigid plan — it's a starting guide that gets refined through the discussions themselves.

### Step 9: Verify

After scaffolding, verify (items marked ★ apply to standard/complex only):

1. **Every hat folder has a CLAUDE.md.**
2. **Root CLAUDE.md has the session start protocol.**
3. **hats.md has a profile for every hat in the hat table.**
4. **Every folder CLAUDE.md's "What does NOT belong here" redirects point to folders that exist.**
5. **Every folder CLAUDE.md's "Read before producing work" references files that exist.** If a referenced file wasn't created as a stub in Step 5 (because the deliverable was uncertain), create it now — any file referenced as a dependency must exist.
6. **The discussion roadmap references deliverable files that exist as stubs.**
7. **Status dashboard is populated.**
8. **`.gitignore` covers OS/IDE files** (and heavy assets + secrets if infrastructure folders were enabled).
9. **If heavy assets were enabled:** `heavy-assets/CLAUDE.md`, `project-tools/CLAUDE.md`, and `project-tools/sync-heavy-assets.spec.md` all exist.
10. ★ **Co-owned folders have per-hat guidance** in their CLAUDE.md (not just one hat's perspective).
11. ★ **`priorities.md` and `cross-hat-dependencies.md` exist** in `0A-ceo/`.
12. **No files exist outside the hat-based folder structure.** If any tool or skill created files in paths like `./docs/superpowers/specs/` or similar external locations, move them into the correct hat folder and delete the external path.

### Step 9b: Multi-Round Audit

After the checklist above passes, run a multi-round audit before committing:

1. **Self-audit** — Re-read every generated governance file with a critical eye. Check for:
   - Internal consistency (do folder CLAUDE.md redirects point to folders that actually exist?)
   - Completeness (are there hats without profiles? folders without governance?)
   - Correctness (do tier assignments match the influence radius logic?)

2. **Sub-agent audit** — Spawn a sub-agent to review the scaffolded structure independently. The sub-agent should:
   - Not receive the original conversation context (fresh eyes)
   - Walk the folder tree and validate each CLAUDE.md against the templates
   - Cross-check hats.md against the actual folder structure
   - Cross-check the discussion roadmap deliverables against existing stubs
   - Report issues with specific file paths and line numbers

3. **Reconciliation** — Compare self-audit and sub-agent findings. Fix all issues. If self and sub-agent disagree, default to the more thorough interpretation.

Only after reconciliation passes: **commit** with a message like: "Bootstrap project structure with hat-based governance"

**Tell the user:** "Project is bootstrapped. Start the next session and the agent will follow the session protocol — read dashboard, load hats, declare a hat, and start Phase 1 of the discussion roadmap."

---

## 3. Reference: Core Concepts

Quick definitions. See the protocol steps for detailed guidance.

- **Hat:** A role/perspective, not a person. Defines what you care about and what you ignore. Switch explicitly. (Details: Step 2)
- **Tier:** Influence radius ordering. Broader decisions = lower tier number. Not chronology. (Details: Step 3)
- **Three-Layer Governance:** Root CLAUDE.md → hats.md → per-folder CLAUDE.md. Agent reads all three. (Details: Step 6)
- **Artifact Ownership:** Each file owned by one hat. Others read, don't edit. Problems filed in cross-hat-dependencies.md. (Details: Section 5)
- **Discussion Roadmap:** Plan for populating stubs through phased discussions. Follows tier ordering. (Details: Step 8)

---

## 4. Reference: Naming Conventions

### Folder names
- Lowercase, hyphenated: `4A-design/`, `6C-build-plan/`
- Singular, not plural: `4A-design/` not `4A-designs/`
- Within numbered tiers, use **single letter** suffix: `4A`, `4B`, `4C`
- For cross-cutting folders outside any tier, use **double letter** prefix: `AA`, `AB`, `AC`
- Double-letter prefixes sort after all single-digit numbers in filesystem (A > 9 in ASCII), so cross-cutting folders appear after hat folders
- Infrastructure folders (heavy-assets, project-tools) get **no prefix** — they sort last alphabetically

### File names
- Lowercase, hyphenated: `problem-statement.md`, `tech-stack.md`
- Descriptive of content, not of the hat: `test-strategy.md` not `qa-plan.md`
- No redundancy with folder name: `1B-roadmap/roadmap.md` is fine, but `1B-roadmap/product-roadmap.md` is redundant

### Stub files
```markdown
# {Title}

> {One-line description of what this file will contain.}
```

---

## 5. Reference: Cross-Hat Rules

### Artifact Ownership

Each file has one owning hat. Other hats read but do not edit.

**When a consuming hat finds a problem:**
1. File in `0A-ceo/cross-hat-dependencies.md`: what's wrong, which file, owning hat, filing hat.
2. Owning hat reviews and fixes.
3. Mark resolved.

### Co-Owned Folders

Some folders have multiple contributing hats (e.g., Specs co-owned by Developer + BA). For these, define a change request split:
- **Technical changes** (schema types, error formats, field names) → technical hat edits directly.
- **Behavioral changes** (scope, user-facing behavior) → non-technical hat approves first. Upstream hats consulted if scope changes.

Document this split in the folder's CLAUDE.md and root CLAUDE.md.

### Plans Lifecycle

Plans subfolders are **optional by default**. Create when:
- A hat begins active work for the first time
- Work is non-obvious or spans multiple sessions

Not every hat needs a written plan.

### Multi-Hat Plans vs Single-Hat Plans

- **Single-hat plans** → `plans/` subfolder inside the hat folder. Example: research methodology lives in `2A-research/plans/`.
- **Multi-hat plans** → own top-level folder at appropriate tier. Example: build plan coordinates Developer + PO + QA, so it gets `6C-build-plan/`.

The test: "Does only one hat read and write this plan?" If yes → subfolder. If multiple hats depend on it → top-level.

### Persistence: Docs Over Memory

All persistent notes, learnings, decisions, and context **must** be written into the project's folder structure — never into agent memory systems (e.g., `~/.claude/projects/*/memory/`). This applies to both the bootstrapping agent and all future agents.

**Why:**
- **Cross-machine portability** — memory is local to one machine/agent instance. Docs are in git, available everywhere.
- **Auditability** — docs are version-controlled and diffable. Memory is opaque.
- **No memory fallacies** — memory degrades across sessions, hallucinates details, and conflates contexts. Docs are stable.

**Where to write instead of memory:**

| What you'd put in memory | Write it here instead |
|---|---|
| Decisions made | `AB-decisions/session-decisions.md` or `AB-decisions/adr/` |
| Session context / what happened | `AA-journal/sessions/{date}.md` |
| Current status / blockers | `0A-ceo/status-dashboard.md` |
| Cross-hat issues | `0A-ceo/cross-hat-dependencies.md` |
| User preferences / project conventions | Root `CLAUDE.md` or relevant folder's `CLAUDE.md` |
| Learnings about the product domain | Relevant hat folder (research, vision, etc.) |

**Rule:** Agents must NOT use memory/recall features as a substitute for proper documentation. If information is worth persisting, it belongs in a governed, version-controlled file.

---

## 6. Reference: Session Protocol

This is what you write INTO the root CLAUDE.md for future sessions. It's not for the bootstrapping session — it's for every session after.

### The Five Steps (Standard/Complex)

1. **Read `0A-ceo/status-dashboard.md`** — current state across all hats.
2. **Read `0A-ceo/hats.md`** — load all hat profiles.
3. **Declare active hat(s)** — state which hat(s) you're wearing. If the user hasn't specified, ask. If neither user nor agent is sure, default to CEO hat for orientation.
4. **Read the CLAUDE.md in your target folder(s)** — understand folder governance before producing work.
5. **Work** — produce artifacts in the correct folders.

### Minimal Variant

For minimal projects, the root CLAUDE.md can embed short hat summaries inline (instead of referencing a separate hats.md), reducing session startup to:

1. **Read root CLAUDE.md** — structure, hat summaries, current state.
2. **Work.**

Even in minimal mode, the agent should still check the status dashboard and declare a hat — just with less ceremony.

### Dashboard Updates

- **Session start:** Verify dashboard is current.
- **Session end:** Update with accomplishments, new blockers, hat status changes.

### End-of-Session Sync Protocol

Documentation drifts from reality as implementation outpaces doc updates. At the end of every session with significant code changes, run a sync pass:

1. **Read `0A-ceo/cross-hat-dependencies.md`** — check each open item against current code. Items implemented but not marked resolved are common (observed: 6 of 11 items already done but undocumented).
2. **Spot-check upstream docs** against implementation — feature-scope.md, use-cases.md, data-model.md, system-design.md are the most likely to drift.
3. **Update docs to match reality.** Mark completed items. Note any new gaps discovered.
4. **Update status dashboard** — Recently Completed, Hat Status, Next Action.
5. **Commit the sync** with a message like "Sync docs with implementation state."

This prevents the "it's already implemented" problem where code is ahead of docs and the next session re-discovers or re-implements completed work.

### Multi-Hat Sessions

Declare all hats at start. When switching, make it explicit: "Switching from Product Owner to Developer hat."

### Multi-Round Auditing Protocol

After completing any significant artifact, phase, or body of work, agents must run a multi-round audit for objectivity and comprehensiveness:

1. **Self-audit** — Re-read your own output critically. Check for:
   - Internal consistency (does it contradict other project docs?)
   - Completeness (are there gaps, vague hand-waves, or missing edge cases?)
   - Correctness (do claims hold up? do references resolve?)
   - Governance compliance (is the artifact in the right folder, following the right template?)

2. **Sub-agent audit** — Spawn a sub-agent to review the output independently. The sub-agent:
   - Must NOT receive the original conversation/reasoning context (fresh perspective)
   - Reviews the artifact as if encountering it for the first time
   - Checks against upstream dependencies listed in the folder's CLAUDE.md
   - Validates against project governance (hat profiles, folder rules, templates)
   - Reports issues with specific file paths, line numbers, and severity

3. **Reconciliation** — Compare self-audit and sub-agent findings:
   - Fix all issues both rounds agree on
   - For disagreements, default to the more conservative/thorough interpretation
   - Document any significant audit findings in `AA-journal/sessions/{date}.md`

**When to audit:**
- After bootstrap verification (Step 9 of bootstrapping protocol)
- After completing a discussion roadmap phase
- After producing any artifact that downstream consumers depend on
- Before committing a batch of changes

**When NOT to audit** (to avoid overhead on trivial work):
- Single-line status dashboard updates
- Adding a stub file
- Fixing a typo

### Docs Over Memory

Agents must write all persistent notes into the project's folder structure, not into agent memory systems. See Section 5 → "Persistence: Docs Over Memory" for the full policy and routing table.

### Autonomous Continuation Protocol

Agents should be able to pick up work and continue without human prompting. This requires machine-readable status.

**The pick-up-work loop:**

```
1. Read 0A-ceo/status-dashboard.md
2. Read the Next Action section — find the highest-priority hat with a pending action
3. Read 0A-ceo/hats.md — load that hat's profile
4. Read the target folder's CLAUDE.md
5. Execute the next action
6. Run quality gates (lint, typecheck, test, build)
7. Update status-dashboard.md:
   - Mark completed work in Recently Completed
   - Set new Next Action for this hat (or clear if phase is done)
   - Advance milestone state if quality gates pass
8. Commit with descriptive message
9. If more actions remain and no blockers: loop to step 1
```

**Next Action field:** The status dashboard must include a `Next Action` section with one entry per hat that has pending work. Each entry is machine-readable:

```markdown
## Next Action

| Priority | Hat | Action | Target | Blocked By |
|----------|-----|--------|--------|------------|
| 1 | Developer | Implement M3 — Claude Code Adapter | 7A-app/ | — |
| 2 | QA | Write integration tests for M2 | 5B-quality/ | — |
| 3 | UX Designer | Create wireframes for Browse tab | 4A-design/wireframes/ | Phase 5 |
```

**Milestone state machine:** Track in `milestones.md`:

```
not-started → in-progress → gates-passing → complete
```

An agent advances state only after running the quality gate commands defined for that milestone. Never mark complete without evidence (test output, build success).

**Quality gate commands:** Define in `implementation-plan.md` per milestone:

```markdown
### M3 — Claude Code Adapter
- **Scope:** ...
- **Quality gate:** `npm run lint && npm run typecheck && npm test -- --filter=adapter`
- **Done when:** All adapter unit tests pass, integration test with fixture config passes
```

### Automated Validation Loop

After all milestones are complete, the CEO hat orchestrates sub-agent validation:

**Protocol:**

```
1. CEO reads all use cases from {requirements folder}/use-cases.md
2. CEO reads all personas from {vision folder}/personas.md
3. For each persona × relevant use case:
   a. Spawn a sub-agent wearing that persona's hat
   b. Sub-agent attempts the use case end-to-end (in the real app/code)
   c. Sub-agent reports: PASS or FAIL with evidence (error, unexpected behavior, missing flow)
4. Collect results into {quality folder}/reports/validation-round-{N}.md
5. Calculate success rate: passed / total scenarios
6. If success rate < 97%:
   a. File each failure as an issue → identify owning hat
   b. Owning hat fixes the issue
   c. Run quality gates to ensure no regressions
   d. Increment N, go to step 3 (re-test ALL scenarios, not just failures)
7. If success rate ≥ 97%: mark validation complete in status dashboard
```

**Validation report format:**

```markdown
# Validation Round {N} — {date}

Success rate: {X}% ({passed}/{total})

| # | Persona | Use Case | Result | Evidence |
|---|---------|----------|--------|----------|
| 1 | Power User | UC-1 View Installed | PASS | — |
| 2 | New Adopter | UC-3 Install | FAIL | Error: adapter not found for scope "local" |
```

**Key rules:**
- Sub-agents get NO conversation context — they test as a fresh user would
- Each round re-tests everything (fixes can introduce regressions)
- The 97% threshold is a default — user can override. For pre-PMF products, 80% may be appropriate. For production tools, 97%+ is recommended.
- Validation is the last gate before marketing/launch phases
- For minimal projects (see Step 0), the validation loop can be simplified to manual testing against the primary persona's use cases — sub-agent orchestration is optional

**Iterative test-fix cycle:** The validation loop in practice runs as a tight cycle:

```
1. User hat sub-agent navigates the live app (via browser tool or Playwright), tests features, captures evidence
2. QA hat analyzes findings, categorizes severity, calculates effectiveness rate
3. Developer hat sub-agents (up to 5 in parallel) implement fixes with file ownership boundaries
4. CEO verifies fixes, audits quality, triggers next cycle
```

Real progression example: Cycle 1 (view-level) 95.5% → Cycle 2 (feature-level) 65.1% → 82.5% → Cycle 3 (targeted) 97.8%. The rate drops when you switch from view-level to feature-level testing — this is expected and healthy. Feature-level tests are more honest.

**Parallel fix dispatch:** When many failures exist, dispatch multiple Developer sub-agents in parallel with strict file ownership:

```
Agent A: Subsystem 1 (owns: files in src/feature-a/)
Agent B: Subsystem 2 (owns: files in src/feature-b/)
Agent C: Subsystem 3 (owns: files in src/feature-c/)
...
```

Each agent owns specific files or directories. No two agents touch the same files. CEO orchestrates and reconciles. Define ownership boundaries by feature directory, module, or file name pattern — whatever creates the cleanest separation for your codebase.

**Simulated usability testing (tier 2):** Before or alongside the validation loop, run a simulated usability test with 3-5 AI personas reading source code and imagining using the app from their persona's perspective:

```
1. Read all persona profiles from {vision folder}/personas.md
2. Read the interaction model / wireframes from {design folder}/
3. Read actual component source code for every screen the personas will encounter
4. Dispatch 3-5 parallel persona agents, each with:
   - Their persona profile (role, technical level, pain points, goals)
   - The full app structure and interaction model
   - Key details from actual source code (the real rendered UI, not just specs)
   - 4-6 realistic scenarios tailored to their persona's goals
   - Structured output format (issue ID, severity, persona impact, suggestion)
5. Synthesize: cross-persona convergence (3+ personas) = highest confidence
6. Fix convergent issues first, then persona-specific issues
```

Report format:
```markdown
# Simulated Usability Test — {date}

| # | Persona | Profile | Issues Found |
|---|---------|---------|-------------|
| 1 | Power User | Senior dev, multiple tools, many components | 23 |
| 2 | Non-Technical User | QA analyst, single tool, no coding | 25 |

## Cross-Persona Convergence (3+ personas)
### 1. {Issue title}
**Flagged by:** {personas}
**Severity:** {CRITICAL/HIGH/MEDIUM/LOW}
{Description and recommended fix}
```

Real example: 5-persona test found 107 issues with 7 cross-persona convergent themes, leading to 16 concrete fixes (keyboard shortcuts, terminology standardization, first-run UX, tooltips).

**What simulated testing catches:** Jargon, unexplained terminology, missing tooltips, broken flows, terminology inconsistency, first-run onboarding gaps, error recovery dead ends.
**What it misses (need live or human testing):** Actual visual rendering, click target responsiveness, timing/animation bugs, motor skill issues, emotional reactions.

**Live agent testing (tier 3):** After simulated testing fixes land, AI persona agents build, launch, and interact with the real running app via Playwright MCP tools. They take screenshots, read the actual rendered UI, click buttons, fill forms, navigate tabs — like a human tester sitting in front of the screen. They see **real pixels**, not source code.

Why this tier exists: A button can look correct in code but render offscreen, overlap another element, or fail to respond to clicks due to z-index/event handler issues. This catches an entire class of bugs invisible to code review.

```
1. Build and launch the app (e.g., npm run build && npm start, or equivalent)
2. Verify the app is running (screenshot + DOM snapshot)
3. Dispatch persona agents SEQUENTIALLY (not parallel — they share one app instance,
   each agent's actions affect the next. Alternatively, relaunch between agents.)
4. Each agent receives:
   - Their persona profile (from {vision folder}/personas.md)
   - Access to Playwright MCP tools (screenshot, click, snapshot, fill, press key)
   - 3-5 task-based scenarios (goal-oriented, not step-by-step)
   - Instructions: "You are a real user. You can ONLY see what's on screen.
     You cannot read source code. Discover features by exploring."
5. Agent interaction loop per task:
   a. Screenshot current state
   b. Read DOM snapshot (accessibility tree)
   c. Decide next action based on persona goals
   d. Perform action (click, type, press key)
   e. Screenshot result
   f. Evaluate: did the expected thing happen? If not → log finding
6. Synthesize findings across all agents
7. Cross-reference with simulated testing — flag issues NOT caught by code review
8. Categorize: Visual | Interaction | Timing | State | Navigation | Accessibility
```

Report format:
```markdown
# Live Agent Test — {date}

Build: {version/commit}  Window: {width}x{height}  Personas: {list}

## Findings
| ID | Persona | Task | Severity | Category | What Happened | Expected |
|----|---------|------|----------|----------|---------------|----------|
| LAT-1 | Power User | Export bundle | HIGH | Interaction | Button unresponsive | File save dialog |

## New Findings (not caught by simulated testing)
{Issues that simulated testing missed because they only manifest visually}

## Comparison: Simulated vs. Live
| Dimension | Simulated (code) | Live Agent (pixels) |
|-----------|-----------------|---------------------|
| Jargon/terminology | Strong | Weak (agent knows jargon) |
| Visual/layout bugs | Cannot see UI | Strong |
| Interaction bugs | Imagines interactions | Actually clicks |
| Timing/animation | No runtime | Real runtime |
```

**Safety for live agent testing:** Some tasks involve destructive actions (install, uninstall, settings changes). For the first run, limit to **read-only exploration** (browse, navigate, search, open panels, screenshot). Destructive actions only against fixture/test data or after user confirmation.

**Recommended overall order:** Simulated (tier 2) → fix → Live agent (tier 3) → fix → Human (tier 4).

Note: Neither simulated nor live agent testing replaces real human testing. Document explicitly: "AI user testing complete. Real human testing recommended before public launch."

### User Testing Protocol (for root CLAUDE.md)

Add this section to the root CLAUDE.md template when personas are defined:

```markdown
### User Testing Protocol

When "user testing" or "do user testing" is requested, run **simulated user testing** (tier 2): spawn 3-5 parallel sub-agents, each role-playing a persona from `{vision folder}/personas.md`. Each agent reads source code and reports issues from their persona's perspective. Reconcile cross-persona.

When "live testing" or "test the real app" is requested, run **live agent testing** (tier 3): build and launch the app, then spawn sequential sub-agents that interact via Playwright MCP tools (screenshot, click, fill, navigate). Agents see real pixels only — no source code. Report visual, interaction, timing, and state issues.

Default trigger "user testing" = simulated (tier 2). Use "live testing" explicitly for tier 3. Run both before release: simulated → fix → live → fix.

All personas from `{vision folder}/personas.md` participate — not just the primary. Optionally add 1-2 ad-hoc personas (skeptical first-timer, non-technical manager). Each reports independently; reconciliation is cross-persona.
```

---

## 7. Examples

### Example A: Minimal — Open Source Library

**4 hats:** CEO, Product Owner, Developer, User

No QA hat (Developer handles testing inline), no UI, no marketing, no research. QA merged into Developer because for a simple library, testing and coding are the same perspective.

```
0A-ceo/
  status-dashboard.md, hats.md, discussion-roadmap.md
  (no priorities.md or cross-hat-dependencies.md — solo project, see Step 0)
1A-vision/
  problem-statement.md, goals.md
2A-requirements/
  use-cases.md
3A-architecture/
  system-design.md, tech-stack.md
4A-app/
AA-journal/
AB-decisions/
```

4 tiers, 7 folders. The minimum viable structure. No specs folder (library is simple enough to implement from architecture docs). No roadmap folder (product direction is in vision). Discussion roadmap has ~5 phases. CEO folder is lean — no priorities or cross-hat-dependencies because there's only one person.

### Example B: Solo Developer Building a CLI Tool

**5 hats:** CEO, Product Owner, Developer, QA, User

No UX Designer (CLI design is Developer's job here), no BA, no Marketing.

```
0A-ceo/
  status-dashboard.md, priorities.md, cross-hat-dependencies.md, hats.md, discussion-roadmap.md
1A-vision/
  problem-statement.md, goals.md
2A-requirements/
  use-cases.md, mvp-definition.md
3A-architecture/
  system-design.md, tech-stack.md
3B-specs/
4A-quality/
  test-strategy.md, edge-cases.md
5A-app/
AA-journal/
AB-decisions/
```

6 tiers (0-5), no design folder, no marketing, no research. Scales down cleanly.

### Example C: Team Building a SaaS Platform

**11 hats:** CEO, PO, BA, UX Designer, Developer, QA, User, Marketing, Repo Ops, Security, DevOps

```
0A-ceo/
1A-vision/
1B-roadmap/
2A-research/
3A-requirements/
4A-design/
4B-architecture/
4C-security/
5A-specs/
5B-quality/
6A-marketing/
6B-rollout/
6C-build-plan/
6D-release/
6E-infrastructure/
7A-app/
AA-journal/
AB-decisions/
```

Security at 4C — peer with design and architecture because security constraints shape both. Release at 6D — Repo Ops manages publish workflow, GitHub releases, community triage. Infrastructure at 6E — peer with rollout (deployment concerns). Repo Ops hat may be added post-MVP when launch approaches rather than during bootstrap.

### Example D: Data-Heavy Analytics Product

**6 hats:** CEO, PO, Data Engineer, Developer, QA, User

```
0A-ceo/
1A-vision/
2A-data-architecture/
  schemas/, pipelines/, data-quality/
3A-requirements/
4A-app-architecture/
4B-specs/
5A-quality/
6A-app/
AA-journal/
AB-decisions/
```

Data Architecture at tier 2 — high radius because the data model shapes everything downstream. App Architecture at tier 4 — builds on top of data model. No UX, no Marketing.

---

## 8. Anti-Patterns to Avoid During Bootstrap

These are mistakes made during scaffolding, not during ongoing work:

### Too Many Hats

**Symptom:** 12 hats defined for a solo project.
**Fix:** Merge hats that never conflict. If "Frontend Developer" and "Backend Developer" never disagree, they're one Developer hat.

### Over-Scaffolding

**Symptom:** 30 stub files and 15 subfolders created "just in case."
**Fix:** Only create stubs for deliverables you can name concretely. If you can't describe what the file will contain, don't create it yet.

### Wrong Tier Assignment

**Symptom:** Architecture at tier 2 (too high — it's not broader than requirements) or Vision at tier 4 (too low — it shapes everything).
**Fix:** Re-run the influence radius question: "If the decisions in this folder change, how many other folders are affected?" Higher impact = lower tier number.

### False Peers

**Symptom:** Two folders at the same tier but one clearly feeds the other with no feedback loop.
**Fix:** If the dependency is one-way, they're at different tiers. Same-tier means co-dependent (both constrain each other) or independent (no dependency).

### Missing Redirects

**Symptom:** Folder CLAUDE.md says "What belongs here" but no "What does NOT belong here."
**Fix:** Every folder CLAUDE.md must have redirects. Think about what an agent might misplace in this folder and add explicit "→ go to X instead" entries.

### Governance Without a Roadmap

**Symptom:** Beautiful folder structure with governance files, but no plan for how to populate the stubs.
**Fix:** Always generate the discussion roadmap (Step 8). The structure is useless without a path to filling it.

### Creating Docs Outside the Project Structure

**Symptom:** Agent creates files in `./docs/superpowers/specs/`, `./docs/`, or any path outside the hat-based folder structure. Common with AI tool skills/plugins that have hardcoded output paths.
**Fix:** All artifacts must live in the project's tiered folder structure. There is no valid reason to create files outside the established structure. If a tool or skill defaults to an external path, override it — move the content into the correct hat folder, delete the external path, and add the external path to `.gitignore` as a safety net.

### Persisting Context in Agent Memory

**Symptom:** Agent saves project decisions, user preferences, or session context to its memory system (`~/.claude/projects/*/memory/` or equivalent) instead of project docs.
**Fix:** All persistent information belongs in the project's version-controlled folder structure. Memory is machine-local, non-auditable, and prone to hallucination-style drift. See Section 5 → "Persistence: Docs Over Memory."

### Milestones Without Quality Gates

**Symptom:** Build plan has milestones but no automated verification commands. Agent marks milestones "complete" based on vibes.
**Fix:** Every milestone must have a quality gate — a concrete command (e.g., `npm test -- --filter=adapter`) that must pass before state advances. No command passes, no state change.

### Skipping Validation

**Symptom:** All milestones complete, agent declares product "done" without testing real user scenarios.
**Fix:** Run the automated validation loop (Section 6) with sub-agents wearing persona hats. Ship only after 97%+ scenario pass rate.

### Stale Docs After Implementation

**Symptom:** Code implements features but cross-hat-dependencies.md, feature-scope.md, or other upstream docs aren't updated. Next session re-discovers or re-implements completed work.
**Fix:** Run the End-of-Session Sync Protocol (Section 6) at the end of every session with code changes. Common finding: 50%+ of "open" cross-hat items are already resolved in code but undocumented. Documentation debt compounds faster than technical debt.

### False Deferrals

**Symptom:** Features deferred from MVP turn out to be essential within one version. The team builds v1.0, then immediately needs to build v1.1 for the same features they deferred.
**Fix:** Stress-test every deferral against scaled personas during Phase 4. Ask: "Would a power user at 2-3x normal scale find this painful?" Common false deferrals: identity resolution (manual mapping doesn't scale past 10 entities), data visualization (tables satisfy the metric but not the user), third-party enrichment (regex links aren't enough — users want titles and statuses). If a "deferred" feature will be needed within one version, it's not deferred — it's just late.

### Expecting Instant Governance Compliance

**Symptom:** First few sessions have cross-folder violations — artifacts landing in wrong places, hat boundaries crossed, redirects ignored.
**Fix:** This is normal. Governance compliance follows a predictable curve: theoretical (sessions 1-2) → enforced (sessions 3-5) → internalized (sessions 10+). Don't panic about early violations. The governance structure self-corrects as agents read folder CLAUDE.md files more consistently. Flag violations but don't over-invest in preventing them during bootstrap.

---

## 9. Post-MVP Lifecycle

The bootstrapping protocol covers idea → MVP. This section covers what happens after MVP ships. These patterns are extracted from projects that used this guide and went through 3-5 post-MVP versions each.

### Version Planning

After MVP ships, the project enters a release cycle. Plan each version as a mini-roadmap:

1. **Collect the backlog.** Sources: deferred features from Phase 4, user testing findings, validation loop failures, cross-hat dependencies filed during build, and new ideas.
2. **Prioritize ruthlessly.** Create a "Fast-Follow Backlog" section in the status dashboard:
   ```markdown
   ## Fast-Follow Backlog (Prioritized)
   | # | ID | Feature | Status |
   |---|----|---------|--------|
   | 1 | USR-06 | Plugin update mechanism | Pending |
   | 2 | USR-03 | Project-scope scanning | Pending |
   ```
3. **Create versioned build plans.** Each version gets its own implementation plan and milestones file (e.g., `v1.1-implementation-plan.md`, `v1.1-milestones.md` in the build plan folder).
4. **Run quality gates per version.** Same milestone state machine (not-started → in-progress → gates-passing → complete). Same audit protocol.

### Adding Hats Post-MVP

New hats often emerge when launch approaches. The most common:

- **Repo Ops** — added when the product needs a public repo, GitHub releases, or community management. Creates a Release/Publish folder at the same tier as Marketing/Rollout.
- **Business Analyst (post-launch)** — added for competitive positioning, market research for v2 direction. May land at a high tier number (e.g., 8A-research) since post-launch research informs future direction rather than constraining current design.

**When adding a hat post-MVP:**
1. Create the hat profile in `0A-ceo/hats.md`.
2. Create the folder with appropriate tier prefix. Post-MVP hats can use higher tier numbers — their influence radius is narrower because the core product is already designed.
3. Create the folder's CLAUDE.md with governance rules.
4. Update root CLAUDE.md's hat table and structure listing.
5. Update the discussion roadmap with new phases if needed.

**Tier re-ordering for post-MVP hats:** Tiers can reflect temporal ordering for hats added after launch. A research folder at tier 8 (after the app at tier 6) is valid if the research informs future versions rather than the current design. The influence radius question still applies, just within the scope of "what does this change affect going forward?"

### Refreshing the Discussion Roadmap

The discussion roadmap doesn't end at MVP. Extend it:

```markdown
## Post-MVP Phases

### Phase 11 — v1.1 Scope & Planning
- **Hats:** Product Owner, Developer
- **Goal:** Prioritize fast-follow backlog, plan v1.1 milestones
- **Deliverables:**
  - `{build-plan folder}/v1.1-implementation-plan.md`
  - `{build-plan folder}/v1.1-milestones.md`

### Phase 12 — Launch Preparation (if Repo Ops/Marketing hats exist)
- **Hats:** Marketing, Repo Ops
- **Goal:** Prepare public repo, marketing materials, release assets
- **Deliverables:**
  - `{marketing folder}/readme-draft.md`
  - `{release folder}/publish-checklist.md`
```

### The "Zero Deferred Features" Metric

A strong signal of guide effectiveness: all three proof projects shipped every feature they planned — zero deferred features remaining. This isn't an accident. The guide's Phase 4 deferral stress-test forces honest assessment of what's truly deferrable. Combined with versioned build plans, features deferred from MVP ship in v1.1 or v1.2 rather than accumulating as permanent backlog.

Track this metric in the status dashboard: how many features were planned, how many shipped, how many remain deferred.

### Documentation-to-Code Ratio

Expect the governance repo to contain far more documentation than code. One proof project had 102,667 lines of markdown and 1,839 lines of app code (56:1 ratio). This is by design — the documentation is the project's institutional memory. The code is the output. Don't be alarmed by the ratio; it means the methodology is working.

---

## 10. Templates

### Root CLAUDE.md

```markdown
# {Project Name}

{One-line project description.}

## Session Start Protocol

Every session must begin with these steps, in order:

1. **Read `0A-ceo/status-dashboard.md`** — understand the current state
2. **Read `0A-ceo/hats.md`** — load hat profiles
3. **Declare active hat(s)** — state which hat(s) you are wearing. If unclear, ask.
4. **Read the CLAUDE.md in your target folder(s)** — understand folder governance
5. **Work** — produce artifacts in the correct folders

Do not start producing work without declaring a hat. The hat determines the lens; the folder determines the output location.

## Project Structure

Organized by hat/role, ordered by influence radius (broadest first). Numbers = hat workspaces, letters = cross-cutting.

```
0A-ceo/                  # {Hat} — {one-line description}
{tier}{letter}-{name}/   # {Hat} — {one-line description}
...
AA-journal/              # Chronological — session logs, changelog
AB-decisions/            # Decision records — session decisions, ADRs
```

### Conventions
- Tier encoding: numbers for hat workspaces (0-9), double letters (AA, AB) for cross-cutting
- Same-tier peers use letter suffixes: `4A-design/`, `4B-architecture/`
- Hat-level plans live as `plans/` subfolders; multi-hat plans are top-level folders
- `0A-ceo/` stays lightweight — dashboard and pointers, not heavy content
- Each folder has its own CLAUDE.md — read it before working there

### Three-Layer Governance
1. **This file** — project structure, conventions, session protocol
2. **`0A-ceo/hats.md`** — role definitions for all hats
3. **Per-folder CLAUDE.md** — folder-specific governance rules

### Cross-Hat Rules

**Artifact ownership:** Each file has one owning hat (the hat whose folder it lives in). Other hats read but do not edit. Problems filed in `0A-ceo/cross-hat-dependencies.md`.

**Spec change requests:** {Include this section only if specs folder is co-owned by multiple hats. If specs has a single owner, delete this subsection entirely. For co-owned specs: "When implementation reveals a spec problem: technical changes (schema types, error formats) → technical hat edits directly. Behavioral changes (scope, user-facing behavior) → non-technical hat approves first, upstream hats consulted if scope changes."}

**Plans:** Optional by default. Consider writing when work is non-obvious or spans multiple sessions.

### Multi-Round Auditing Protocol

After completing any significant artifact or phase, run a multi-round audit:

1. **Self-audit** — Re-read your output critically. Check internal consistency, completeness, correctness, and governance compliance.
2. **Sub-agent audit** — Spawn a sub-agent (without original conversation context) to review independently. It checks against upstream dependencies, project governance, and templates, reporting issues with file paths and line numbers.
3. **Reconciliation** — Fix all agreed issues. For disagreements, default to the more thorough interpretation. Document significant findings in `AA-journal/sessions/`.

Skip auditing for trivial changes (status updates, typo fixes, stub additions).

### Docs Over Memory

All persistent notes, decisions, and context must be written into the project folder structure — never into agent memory systems. Memory is machine-local, non-auditable, and prone to drift.

| Instead of memory... | Write to... |
|---|---|
| Decisions | `AB-decisions/` |
| Session context | `AA-journal/sessions/` |
| Status / blockers | `0A-ceo/status-dashboard.md` |
| Cross-hat issues | `0A-ceo/cross-hat-dependencies.md` |
| Conventions | Root or folder `CLAUDE.md` |

### Autonomous Continuation

Agents can work without human prompting by following the pick-up-work loop:

1. Read `0A-ceo/status-dashboard.md` → find the Next Action table
2. Take the highest-priority unblocked action
3. Execute, run quality gates, update dashboard
4. If more actions remain and no blockers: continue

Always update the Next Action table at session end so the next agent (or next session) can continue seamlessly.

### User Testing Protocol

When "user testing" or "do user testing" is requested, run **simulated user testing** (tier 2): spawn 3-5 parallel sub-agents, each role-playing a persona from `{vision folder}/personas.md`. Each agent reads source code and reports issues from their persona's perspective. Reconcile cross-persona.

When "live testing" or "test the real app" is requested, run **live agent testing** (tier 3): build and launch the app, then spawn sequential sub-agents that interact via Playwright MCP tools (screenshot, click, fill, navigate). Agents see real pixels only — no source code. Report visual, interaction, timing, and state issues.

Default trigger "user testing" = simulated (tier 2). Use "live testing" explicitly for tier 3. Run both before release: simulated → fix → live → fix.

All personas from `{vision folder}/personas.md` participate — not just the primary. Optionally add 1-2 ad-hoc personas (skeptical first-timer, non-technical manager). Each reports independently; reconciliation is cross-persona.

### End-of-Session Sync

At the end of every session with significant code changes, run a sync pass:
1. Read all open items in `0A-ceo/cross-hat-dependencies.md` — verify each against current code.
2. Spot-check upstream docs (feature-scope, use-cases, data-model, system-design) against implementation.
3. Update docs to match reality. Mark completed items.
4. Update status dashboard — Recently Completed, Hat Status, Next Action.
5. Commit the sync.

### Output Location

All artifacts must live in this project's folder structure. Never create files in external paths (e.g., `./docs/superpowers/specs/`). If a tool defaults to an external path, move the content into the correct hat folder and delete the external path.

## Hats

We wear {N} hats (including CEO). Full profiles in `0A-ceo/hats.md`.

| Hat | Primary folders |
|-----|----------------|
| **{Hat}** | `{folder}/` |

## Start Here

- [Status Dashboard](0A-ceo/status-dashboard.md) — current state across all hats
- [Hat Profiles](0A-ceo/hats.md) — role definitions for all hats
- [Discussion Roadmap](0A-ceo/discussion-roadmap.md) — phased discovery plan
- [Session Decisions](AB-decisions/session-decisions.md) — cross-session decision log
```

### hats.md (complete file)

```markdown
# Hat Profiles

> Read this file at the start of every session. It defines the mindset, responsibilities, and boundaries for each hat.

When wearing a hat, adopt its perspective fully. Don't mix concerns — if you notice work that belongs to another hat, note it in `0A-ceo/cross-hat-dependencies.md` and move on.

---

## {Hat Name}

> {One-line role summary}

- **Mindset:** {Thinking orientation, not task list}
- **Cares about:** {Focus areas}
- **Does NOT:** {What's left to others — name which hat}
- **Quality bar:** {Testable criteria}
- **Primary folders:** `{folder}/`

---

{...repeat for all hats...}
```

### Per-Folder CLAUDE.md

```markdown
# {tier}{letter}-{folder-name} — {Short Description}

## Contributing hats

### As {Hat 1}
- {What this hat does here}
- {Specific guidance}

### As {Hat 2} (if multi-hat folder)
- {What this hat does here}

## What belongs here
- {Artifact — description}

## What does NOT belong here
- {Misplaced artifact} → `{correct-folder}/`

## Subfolders (include if folder has subfolders)
- `{name}/` — {what it contains}

## Read before producing work
- `{path}` — {why}

## Downstream consumers
- `{folder}/` — {how they use this}
```

### Status Dashboard

```markdown
# Status Dashboard

> Current state across all hats.
> Last updated: {date}

## Current Phase
{Phase from discussion roadmap}

## Current Milestone
{Milestone ID} — {name} | State: {not-started / in-progress / gates-passing / complete}

## Next Action

| Priority | Hat | Action | Target | Blocked By |
|----------|-----|--------|--------|------------|
| 1 | {Hat} | {Specific action an agent can execute} | {folder/file} | {Blocker or —} |

## Hat Status

| Hat | Status | Current Focus | Blocked By |
|-----|--------|---------------|------------|
| {Hat} | {Active / On Hold / Not Started / Complete} | {Focus} | {Blocker or —} |

## Milestones

| ID | Name | State | Quality Gate |
|----|------|-------|-------------|
| M0 | Scaffold | complete | `npm run build` passes |
| M1 | {name} | not-started | `{gate command}` |

## Blockers

| Blocker | Affects | Resolution |
|---------|---------|------------|
| {Description} | {Hats} | {How to resolve} |

## Recently Completed
- {What got done}

## Validation Status
{Not started / Round N — X% success rate / Complete — 97%+}
```

### Cross-Hat Dependencies

```markdown
# Cross-Hat Dependencies

> Filed by consuming hats. Resolved by owning hats.

## Open

| Filed By | Owning Hat | File | Issue | Date Filed |
|----------|-----------|------|-------|------------|

## Resolved

| Filed By | Owning Hat | File | Issue | Resolution | Date Resolved |
|----------|-----------|------|-------|------------|---------------|
```

### Discussion Roadmap

```markdown
# Discussion Roadmap

> Established: {date}
> Status: Not Started — Phase 1

{Brief description of what the discussion phases are for.}

## Folder Map

| Folder | Hat | Contains |
|--------|-----|----------|
| `{folder}/` | {Hat} | {One-line description} |

## Phases

### Phase 1 — {Title}
- **Hats:** {active hats}
- **Goal:** {one line}
- **Deliverables:**
  - `{path/to/file.md}`
- **Depends on:** None

### Phase 2 — {Title}
- **Hats:** {active hats}
- **Goal:** {one line}
- **Deliverables:**
  - `{path/to/file.md}`
- **Depends on:** Phase 1 (BLOCKING — reason, if hard dependency)

{...continue...}

## Progress Tracker

| Phase | Status | Date Started | Date Completed |
|-------|--------|--------------|----------------|
| 1. {Title} | Not Started | | |
```

### Session Decisions Log

```markdown
# Session Decisions Log

Decisions made during discussions that affect future sessions.

## {Date} — {Session Topic}

| Decision | Context |
|----------|---------|
| {What was decided} | {Why, and any relevant links} |
```

### ADR (Architecture Decision Record)

```markdown
# ADR-{number}: {Title}

## Status
{Proposed / Accepted / Superseded by ADR-XXX}

## Context
{What situation prompted this decision. What constraints exist.}

## Options Considered
1. **{Option A}** — {pros and cons}
2. **{Option B}** — {pros and cons}

## Decision
{What we chose and why}

## Consequences
- {Positive consequence}
- {Negative consequence or trade-off}
```

### Changelog (in AA-journal/)

```markdown
# Changelog

> Version-oriented record of what changed at each milestone. Not every session gets an entry — only when a meaningful milestone is reached.

## {version or milestone name} — {date}
- {What changed}
- {What was added}
```

### Priorities (in 0A-ceo/)

```markdown
# Priorities

> What matters most right now and where to focus.
> Last updated: {date}

## Current Focus
1. {Highest priority item}
2. {Second priority}

## Parked (important but not now)
- {Item} — {why it's parked}
```

### Session Log (in AA-journal/sessions/)

```markdown
# {YYYY-MM-DD} — {Topic}

## Hats active
{list}

## Accomplished
- {what got done}

## Unfinished / Blocked
- {what's left}

## Decisions made
- {decision} (see decisions folder if formal)
```

### Heavy Assets CLAUDE.md

```markdown
# heavy-assets — Large Files (Not in Git)

> This folder is gitignored. Synced via {sync method}.

## Structure
Mirrors the hat folder structure.

## What belongs here
- Videos, screen recordings
- Large images (hi-res, design exports)
- Design source files (Figma, PSD, Sketch)
- Any file too large or binary for git

## What does NOT belong here
- Text documents → hat folders in git
- Source code → {app folder}
- Small images (icons, SVGs) → {design folder}/assets/

## Setup

Machine-local config in `.heavy-assets.secret` (root, gitignored).
Sync via `project-tools/sync-heavy-assets.sh`.
```

### Project Tools CLAUDE.md

```markdown
# project-tools — Project Infrastructure Tooling

> Tools that support working on the project, not the product itself.

## What belongs here
- Sync scripts (heavy-assets ↔ cloud storage)
- Setup scripts (new machine bootstrapping)
- Development workflow helpers
- Any script that operates on the project structure

## What does NOT belong here
- Product source code → {app folder}
- Build or CI scripts for the product → {app folder}
- Documentation → hat folders
- Secrets or machine-local config → root dotfiles (gitignored)

## Conventions
- Scripts should be self-documenting (usage info on `--help`)
- Machine-specific config goes in root dotfiles (e.g., `.heavy-assets.secret`), not here
- This folder is committed to git — no secrets
```

### Sync Heavy Assets Spec (`project-tools/sync-heavy-assets.spec.md`)

Don't embed the full spec here — generate it as a standalone file. The spec should cover:

- **Purpose:** Interactive tool to sync `heavy-assets/` with the user's chosen cloud storage (from Step 1 decision gate)
- **Core operations:** Compare (diff local vs cloud), Push (local → cloud), Pull (cloud → local)
- **Auth:** Support OAuth and service account modes; credentials stored in `.heavy-assets.secret` (root, gitignored)
- **First-run setup:** Interactive flow to configure auth and cloud folder ID
- **Safety:** Always show dry-run preview before changes; deletion is opt-in with separate confirmation; skip `CLAUDE.md` and `.gitkeep` during sync
- **Implementation:** Recommend rclone wrapper as simplest approach; fall back to language-specific SDK if needed
- **Platform:** Cross-platform (Windows Git Bash, macOS, Linux)
