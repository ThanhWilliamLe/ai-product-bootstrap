# AI Trends Weekly

Weekly AI trends newsletter for tech-curious professionals. Substack-published, automation-assisted.

## Goals

- G1: Content pipeline — repeatable process from topic research to published issue
- G2: Research automation — scripts that surface candidate topics from RSS feeds
- G3: Publishing automation — script to push finished issues to Substack via API
- G4: Quality content — every issue is well-sourced, clearly written, under 1500 words

## State

Version: 0.1
Phase: foundation
Last shipped: — (none yet)

## Queue

### Foundation

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Set up package.json with dotenv, test runner, project metadata | G2, G3 | build | — | ready |
| 2 | Create content calendar template in docs/content/calendar.md — weekly cadence, Sunday publish, 4-week lookahead | G1 | content | — | ready |
| 3 | Write style guide in docs/content/style-guide.md — voice, audience, word limit, source rules, format | G4 | content | — | ready |

### Automation

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 4 | Build Substack publish script (src/publish-to-substack.js) — push markdown to Substack API, handle draft/publish modes | G3 | build | 1 | — |
| 5 | Build topic scout script (src/topic-scout.js) — pull from RSS feeds, rank by recency and relevance, output candidate list | G2 | build | 1 | — |
| 6 | Write tests for publish and scout scripts — happy path, error handling, mock API responses | G2, G3 | verify | 4, 5 | — |

### First Content Cycle

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 7 | Research 3 candidate topics for Issue #1 — use scout output + manual scan of RSS sources | G1 | research | 5 | — |
| 8 | Deep-dive research on selected topic — gather 3+ sources, fact-check claims, note angles | G4 | research | 7 | — |
| 9 | Draft Issue #1 — follow style guide, under 1500 words, cite all sources | G1, G4 | content | 3, 8 | — |
| 10 | Edit and polish Issue #1 — check voice, cut fluff, verify links, run format checker | G4 | content | 9 | — |
| 11 | Publish Issue #1 to Substack — use publish script, verify live rendering | G1, G3 | content | 4, 10 | — |

### Refinement

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 12 | Build format checker script (src/format-check.js) — word count, link validation, required sections present | G4 | build | 1 | — |
| 13 | Seed content backlog with 10+ future topic ideas from research | G1 | content | 7 | — |
| 14 | Retrospective on Issue #1 process — what worked, what was slow, what to automate next | G1 | discover | 11 | — |
| 15 | Update calendar with Issues #2-5 topics and tentative dates | G1 | content | 13, 14 | — |

## Decisions

No decisions yet. Foundation phase — decisions will be recorded here as they're made.

## Done

(empty)
