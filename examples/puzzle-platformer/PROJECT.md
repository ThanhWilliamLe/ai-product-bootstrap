# Puzzle Platformer

2D pixel-art puzzle platformer. PC + mobile (Android/iOS). Solo indie dev building in Unity with C#.

## Goals

- G1: Playable core loop — player moves, solves puzzles, completes levels. This is the game.
- G2: Multi-platform input — keyboard and touch controls feel native on both PC and mobile.
- G3: Level progression — players advance through levels with increasing difficulty.
- G4: 5 shippable levels — enough content to validate the full experience end-to-end.
- G5: Clean mobile build — Android and iOS builds run without errors or performance issues.

## State

Version: 0.1
Phase: building
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Set up Unity project scaffold (folders, assembly definitions, .gitignore, packages) | G1 | build | — | ready |
| 2 | Design core mechanics: movement, jump, puzzle interactions, hazards | G1 | design | — | ready |
| 3 | Implement player controller (Rigidbody2D movement, jump, ground check) | G1 | build | 1, 2 | — |
| 4 | Implement puzzle interaction system (triggers, switches, doors, push blocks) | G1 | build | 3 | — |
| 5 | Design input abstraction: actions for keyboard + touch, virtual joystick spec | G2 | design | — | ready |
| 6 | Implement cross-platform input manager (Input System package, touch + keyboard) | G2 | build | 1, 5 | — |
| 7 | Design game loop: scene flow, level select, pause, win/lose conditions | G3 | design | — | ready |
| 8 | Implement scene management and level progression (scene loading, save progress) | G3 | build | 1, 7 | — |
| 9 | Implement game UI (HUD, pause menu, level select, win/lose screens) | G3 | build | 8 | — |
| 10 | Design 5 levels: tile grids, puzzle placement, difficulty curve | G4 | design | 2 | — |
| 11 | Implement level data loader (ScriptableObject-based level configs, tile spawner) | G4 | build | 4 | — |
| 12 | Build all 5 levels from design specs | G4 | build | 10, 11 | — |
| 13 | Write unit tests (player controller, puzzle logic, scene management) | G1 | verify | 4, 8 | — |
| 14 | Create build script (BuildScript.cs, batch mode build for PC/Android/iOS) | G5 | build | 1 | — |
| 15 | Mobile QA: integration tests, batch build validation, touch input testing | G5 | verify | 6, 12, 14 | — |

## Decisions

- D1: Built-in physics only (Rigidbody2D, Collider2D) — keep it simple, no third-party dependencies — constrains: all movement and collision code
- D2: Pixel art created by human developer, not agents — agents never generate or modify art assets — constrains: art pipeline, agent scopes
- D3: No multiplayer — solo experience only, no netcode — constrains: architecture, no networking layer

## Done

(none yet)
