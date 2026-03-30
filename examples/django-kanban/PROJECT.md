# TaskFlow — Kanban + Real-Time Notifications

Django 5 task management API (~8,000 lines) adding WebSocket notifications and a React/TypeScript Kanban board.

## Goals

- G1: Real-time notifications — users see task updates instantly via WebSocket without polling
- G2: Kanban frontend — React/TypeScript board for visual task management, replacing API-only workflow
- G3: Email notifications deferred — documented as out-of-scope for this version cycle

## State

Version: 0.1
Phase: building
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Audit existing codebase: map models, serializers, views, auth flow | G1, G2 | discover | — | ready |
| 2 | Choose frontend package manager (npm vs pnpm vs yarn) | G2 | decide | — | ready |
| 3 | Choose frontend state management (Zustand vs Redux Toolkit vs React Query) | G2 | decide | — | ready |
| 4 | Define WebSocket message schema (event types, payload shapes) | G1 | design | 1 | — |
| 5 | Design Notification model and API endpoints | G1 | design | 1 | — |
| 6 | Design Kanban board component hierarchy and data flow | G2 | design | 1, 3 | — |
| 7 | Install and configure Django Channels + Redis channel layer | G1 | build | 4 | — |
| 8 | Build Notification model, serializer, and DRF endpoints | G1 | build | 5 | — |
| 9 | Build WebSocket consumer for notifications | G1 | build | 7, 8 | — |
| 10 | Emit notification events on task create/update/delete/assign | G1 | build | 9 | — |
| 11 | Scaffold React + TypeScript frontend with build tooling | G2 | build | 2 | — |
| 12 | Build Kanban board UI (columns, cards, drag-and-drop) | G2 | build | 6, 11 | — |
| 13 | Connect WebSocket to frontend for live updates | G1, G2 | build | 10, 12 | — |
| 14 | Unit tests: Notification model, consumer, API, React components | G1, G2 | verify | 10, 12 | — |
| 15 | Integration tests: WS auth, end-to-end notification flow, Kanban API round-trip | G1, G2 | verify | 13 | — |

## Pending Decisions

- D1: Frontend package manager — npm, pnpm, or yarn? Affects lockfile, CI caching, monorepo support. Constrains: frontend scaffold (item 11), CI config.
- D2: Frontend state management — Zustand (lightweight), Redux Toolkit (ecosystem), or React Query (server-state focus)? Constrains: component data flow (item 6), WebSocket integration pattern (item 13).
- D3: WebSocket auth strategy — pass JWT as query param on connect, or use ticket-based auth? Constrains: consumer middleware (item 9), frontend connect logic (item 13).

## Done

(none yet)
