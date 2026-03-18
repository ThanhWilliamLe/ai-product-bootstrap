# Bootstrapping a Product Development Project with AI Agents

> This guide is for AI agents. A user gives you this file and asks you to bootstrap their project. Follow the protocol below.

> This guide is standalone. It is not specific to any product or technology.

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
9. [Templates](#9-templates)

---

## 1. How to Use This Guide

### Who reads this

An AI agent, given this file by a user who wants to bootstrap a new product development project.

### What you'll produce

By the end of the protocol, the project will have:
- A hat-based folder structure with governance files
- Hat profiles defining each role
- Per-folder CLAUDE.md files with ownership rules and redirects
- A root CLAUDE.md with session protocol
- An initialized CEO dashboard
- A discussion roadmap for phased discovery across future sessions
- Cross-cutting folders (journal, decisions)
- Infrastructure folders if needed (heavy assets, project tools)

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
- Are there heavy assets (videos, design files, large images)?
- Any existing work to incorporate?

**Don't ask yet:** technical stack, architecture details, feature lists. Those come later during the discussion phases, not during bootstrapping. If the user volunteers this info unprompted, acknowledge it and note it for the architecture phase — don't ignore it, but don't let it drive bootstrapping decisions.

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
| **Data Engineer** | Data pipelines, schemas, ETL | Data-heavy product |
| **Security** | Threat modeling, auth, compliance | Sensitive data or attack surface |
| **DevOps/Platform** | CI/CD, infrastructure, deployment | Complex deployment needs |

**Decision logic:**
- Start with 4 mandatory: CEO, Product Owner, Developer, User.
- Add QA if quality matters (almost always yes).
- Add UX Designer if there's a user interface.
- Add BA if there's an ecosystem to research or a market to analyze.
- Add Marketing if the product will be public.
- Add others based on product specifics.
- Aim for 4-8 hats. Fewer than 4 = probably merging perspectives that should be separate. More than 8 = probably splitting too fine.

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
Tier 6: Marketing, Rollout, Build Plan (downstream)
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

| Hat | Typical deliverables | Typical subfolders |
|-----|---------------------|-------------------|
| CEO | status-dashboard.md, priorities.md, cross-hat-dependencies.md, hats.md, discussion-roadmap.md | — |
| Product Owner | problem-statement.md, product-goals.md, personas.md | — |
| Product Owner (Roadmap) | roadmap.md, release-planning.md | — |
| Business Analyst | ecosystem-research.md, competitive-landscape.md, format-mapping.md | comparisons/, diagrams/, data/, plans/ |
| UX Designer | interaction-model.md | wireframes/, mockups/, prototypes/, assets/, plans/ |
| Developer (Architecture) | data-model.md, tech-stack.md, system-design.md | diagrams/ |
| Developer (Specs) | (populated just-in-time — one spec per feature) | — |
| Developer (Build Plan) | implementation-plan.md, milestones.md | — |
| Developer (App) | (source code — structure TBD) | — |
| QA | test-strategy.md, edge-cases.md | reports/, plans/ |
| Marketing | — | copy/, collateral/, screenshots/, plans/ |
| Rollout | distribution-strategy.md, first-run-experience.md | — |

**Don't over-scaffold.** If a folder will only have 2-3 files, it doesn't need subfolders. If a deliverable is uncertain, don't create a stub — it can be added later. However, if a folder CLAUDE.md's "Read before producing work" will reference a file, that file must exist as a stub at minimum.

**Determine folder ownership:** Most folders are owned by one hat. Some folders have multiple contributing hats. Decide co-ownership when:
- Two hats produce different types of artifacts in the same folder (e.g., Requirements: PO defines scope, BA analyzes feasibility)
- The folder's content requires both a technical and non-technical perspective (e.g., Specs: Developer writes schemas, BA validates against requirements)

For co-owned folders, document the split in the folder's CLAUDE.md: what each hat does there, and for change requests, which type of change each hat can make (see Section 5: Co-Owned Folders).

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
4. Create infrastructure folders if needed (`heavy-assets/`, `project-tools/`) — no prefix.
5. Create all subfolders within hat folders.
6. Add `.gitkeep` to empty subfolders (remove once real files are added).
7. Create all stub files with title + one-line description:
   ```markdown
   # {Title}

   > {One-line description of what this file will contain.}
   ```
8. Set up `.gitignore`:
   ```
   # Heavy assets (if applicable)
   heavy-assets/*
   !heavy-assets/CLAUDE.md

   # Machine-local config
   .heavy-assets.config

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

The project entry point. Use the template in Section 9.

Includes: session start protocol, structure listing, conventions, three-layer governance description, cross-hat rules, hat-to-folder table, start-here links.

**Do not put in root CLAUDE.md:** current status (goes in CEO dashboard), detailed hat profiles (goes in hats.md), technology decisions (goes in architecture folder), anything that changes frequently.

### Step 7: Initialize CEO Dashboard

Populate `0A-ceo/status-dashboard.md` with the initial project state:
- Current phase: "Project bootstrapped. Ready for Phase 1 of discussion roadmap."
- All hats listed with status "Not Started" (except CEO: "Active").
- No blockers (unless the user mentioned constraints).
- Recently completed: "Project structure scaffolded."

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
  Note: Can run in parallel with Phase 2
  BLOCKING: If research reveals the ecosystem is different than assumed,
  Phase 4+ cannot start until research is verified. Mark as hard blocker.

Phase 4: Feature Scope & MVP
  Hats: Product Owner, Business Analyst (if present)
  Goal: Define MVP boundary — what ships, what doesn't
  Populates: {requirements folder} — feature-scope.md, mvp-definition.md
  Depends on: Phases 1, 2, (3 if BA exists — hard blocker if research is foundational)

Phase 5: UX Flows & Interaction Design (if UX Designer hat exists)
  Hats: UX Designer, User
  Goal: Define how users interact with the product
  Populates: {design folder} — interaction-model.md, wireframes/
  Depends on: Phases 2, 4

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
  Goal: Define what to test, how, acceptance criteria
  Populates: {quality folder} — test-strategy.md, edge-cases.md
  Depends on: Phases 4, 6

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
  Hats: Developer, Product Owner
  Goal: Assemble the implementation plan from all prior phases
  Populates: {build-plan folder} — implementation-plan.md, milestones.md
  Depends on: All above phases
```

#### How to customize the roadmap

- **Skip phases** for hats that don't exist (no BA → skip Phase 3).
- **Merge phases** that are small enough to cover in one session.
- **Split phases** that are too large (Phase 7 often becomes multiple phases for complex products). When splitting, use decimal numbering (Phase 7.1, 7.2) or insertion numbering (Phase 7, Phase 7.5, Phase 8).
- **Add phases** for project-specific concerns (security review, data architecture, compliance).
- **Reorder** if the project has unusual dependency structure.

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

Create `0A-ceo/discussion-roadmap.md` with a Folder Map (which hat owns which folder), then the phases and progress tracker. Use the template in Section 9:

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
8. **`.gitignore` covers heavy assets, secrets, OS/IDE files.**
9. ★ **Co-owned folders have per-hat guidance** in their CLAUDE.md (not just one hat's perspective).
10. ★ **`priorities.md` and `cross-hat-dependencies.md` exist** in `0A-ceo/`.

**Then commit** with a message like: "Bootstrap project structure with hat-based governance"

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

### Multi-Hat Sessions

Declare all hats at start. When switching, make it explicit: "Switching from Product Owner to Developer hat."

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

5 tiers, no design folder, no marketing, no research. Scales down cleanly.

### Example C: Team Building a SaaS Platform

**10 hats:** CEO, PO, BA, UX Designer, Developer, QA, User, Marketing, Security, DevOps

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
6D-infrastructure/
7A-app/
AA-journal/
AB-decisions/
```

Security at 4C — peer with design and architecture because security constraints shape both. Infrastructure at 6D — peer with rollout (deployment concerns).

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
**Fix:** Re-run the influence radius question: "If this hat changes its mind, how many other hats are affected?" Higher impact = lower tier number.

### False Peers

**Symptom:** Two folders at the same tier but one clearly feeds the other with no feedback loop.
**Fix:** If the dependency is one-way, they're at different tiers. Same-tier means co-dependent (both constrain each other) or independent (no dependency).

### Missing Redirects

**Symptom:** Folder CLAUDE.md says "What belongs here" but no "What does NOT belong here."
**Fix:** Every folder CLAUDE.md must have redirects. Think about what an agent might misplace in this folder and add explicit "→ go to X instead" entries.

### Governance Without a Roadmap

**Symptom:** Beautiful folder structure with governance files, but no plan for how to populate the stubs.
**Fix:** Always generate the discussion roadmap (Step 8). The structure is useless without a path to filling it.

---

## 9. Templates

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

## Hat Status

| Hat | Status | Current Focus | Blocked By |
|-----|--------|---------------|------------|
| {Hat} | {Active / On Hold / Not Started} | {Focus} | {Blocker or —} |

## Blockers

| Blocker | Affects | Resolution |
|---------|---------|------------|
| {Description} | {Hats} | {How to resolve} |

## Recently Completed
- {What got done}
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
```
