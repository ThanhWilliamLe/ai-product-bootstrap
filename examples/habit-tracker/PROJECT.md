# Streak — Habit Tracker

Mobile-first habit tracker with streaks, reminders, and simple analytics. Solo dev, React Native + Expo, local SQLite storage.

## Goals

- G1: Habit CRUD — users can create, read, update, and delete habits
- G2: Streak tracking — calculate and display current/best streaks to motivate consistency
- G3: Reminders — push notifications at user-chosen times so habits aren't forgotten
- G4: Analytics — simple charts showing completion rates and trends over time
- G5: Polish — smooth UX, consistent design, fast launch, no jank

## State

Version: 0.1
Phase: building
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Scaffold Expo project with TypeScript strict, ESLint, Jest | G1 | build | — | ready |
| 2 | Design DB schema: habits, completions, reminders tables | G1 | build | 1 | — |
| 3 | Design screens: habit list, add/edit, detail, analytics | G5 | design | 1 | — |
| 4 | Implement habit CRUD with expo-sqlite | G1 | build | 2 | — |
| 5 | Build habit list screen with swipe actions | G1 | build | 3, 4 | — |
| 6 | Implement streak calculation logic | G2 | build | 4 | — |
| 7 | Build streak display: current streak, best streak, calendar | G2 | build | 5, 6 | — |
| 8 | Design analytics wireframes: completion rate, weekly trends | G4 | design | 3 | — |
| 9 | Set up expo-notifications for local push notifications | G3 | build | 1 | — |
| 10 | Build reminder UI: per-habit time picker, toggle on/off | G3 | build | 5, 9 | — |
| 11 | Build analytics screen with charts | G4 | build | 6, 8 | — |
| 12 | Write unit tests for CRUD, streaks, and reminder scheduling | G1, G2 | verify | 6, 10 | — |
| 13 | Full QA pass: edge cases, empty states, accessibility | G5 | verify | 7, 10, 11 | — |

## Decisions

- D1: Expo managed workflow — avoids native build complexity for solo dev — constrains: no bare native modules
- D2: expo-sqlite for local storage — no backend, no sync, all data on-device — constrains: no cloud features
- D3: No backend — simplifies architecture, ships faster, user owns their data — constrains: no multi-device sync

## Done

(empty)
