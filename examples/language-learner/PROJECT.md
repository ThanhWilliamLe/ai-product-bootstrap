# LangFlash

AI-powered language learning with flashcards and AI-generated contextual sentences. For solo learners who want practice beyond rote memorization.

## Goals

- G1: Define the product — nail down what we're building, for whom, and why it's different
- G2: Choose the tech stack — pick languages, frameworks, and tools that fit a solo dev building an MVP
- G3: Ship a working MVP — flashcard UI + AI sentence generation, end to end
- G4: Validate with real users — get the app in front of learners and measure whether it helps

## State

Version: 0.1
Phase: discovery
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Define core problem and value proposition — why AI flashcards, what gap exists | G1 | discover | — | ready |
| 2 | Identify target users — who specifically, what languages, what level | G1 | discover | — | ready |
| 3 | Research competitors — Anki, Duolingo, Quizlet, AI-native tools | G1 | research | — | ready |
| 4 | Define MVP scope — minimum feature set to validate the idea | G1 | decide | 1, 2, 3 | — |
| 5 | Choose tech stack — frontend, backend, AI API, data storage | G2 | decide | 4 | — |
| 6 | Design core UX — flashcard flow, sentence display, review loop | G3 | design | 4 | — |
| 7 | Scaffold project — repo structure, build config, CI basics | G3 | build | 5 | — |
| 8 | Determine verification commands — lint, test, build, typecheck | G2 | decide | 5 | — |
| 9 | Implement AI sentence generation — prompt design, API integration | G3 | build | 5, 6 | — |
| 10 | Implement flashcard UI — card display, flip, rate, next | G3 | build | 6, 7 | — |
| 11 | Integrate end-to-end flow — generate sentence → display card → user rates → next | G3 | build | 9, 10 | — |
| 12 | Write tests — unit and integration coverage for core flows | G3 | verify | 11 | — |
| 13 | Plan user validation — how to get it in front of learners, what to measure | G4 | discover | 11 | — |

## Decisions

No decisions yet. Discovery phase — decisions will be recorded here as they're made.

## Done

(empty)
