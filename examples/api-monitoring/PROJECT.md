# PingBoard

API monitoring SaaS. Scheduled endpoint checks, uptime/latency tracking, alerting (email + Slack), and a real-time dashboard. Built for dev teams and DevOps engineers.

## Goals

- G1: MVP monitoring — users can add API endpoints and see check results
- G2: Scheduled checks — cron-based endpoint checking via BullMQ job queues
- G3: Alerting — notify via email and Slack when endpoints go down or degrade
- G4: Dashboard — real-time uptime/latency charts and status overview
- G5: Multi-tenant auth — team-based access with invite flow and role separation

## State

Version: 0.1
Phase: building
Last shipped: — (none yet)

## Queue

| # | Item | Goal | Type | Depends | Status |
|---|------|------|------|---------|--------|
| 1 | Scaffold monorepo (apps/web, apps/api, packages/shared) with TS strict, ESLint, Prettier | G1 | build | — | ready |
| 2 | Design DB schema: endpoints, checks, incidents, users, teams | G1 | build | 1 | — |
| 3 | Research BullMQ patterns for recurring cron jobs with retries and backoff | G2 | research | — | ready |
| 4 | Implement endpoint checker service: HTTP checks with timeout, status, latency | G2 | build | 1, 2 | — |
| 5 | Build REST API: CRUD endpoints, check history, incident list | G1 | build | 2, 4 | — |
| 6 | Create dashboard wireframes: status overview, uptime chart, latency chart, endpoint list | G4 | design | — | ready |
| 7 | Build dashboard pages from wireframes: overview, endpoint detail, incident timeline | G4 | build | 5, 6 | — |
| 8 | Research Slack webhook API and email transactional providers (Resend, Postmark, SES) | G3 | research | — | ready |
| 9 | Build alert engine: threshold rules, cooldown, fan-out to email + Slack channels | G3 | build | 4, 8 | — |
| 10 | Implement auth: NextAuth.js, team model, invite flow, RBAC (admin/member/viewer) | G5 | build | 2 | — |
| 11 | E2E verification: add endpoint, wait for check, trigger alert, verify dashboard | G1 | verify | 7, 9, 10 | — |
| 12 | CI pipeline: lint, typecheck, test, build in GitHub Actions | G1 | build | 1 | — |

## Decisions

- D1: Monorepo with apps/web + apps/api + packages/shared — keeps frontend, backend, and shared types in one repo with clear boundaries. Constrains: all imports from shared go through packages/shared.
- D2: TypeScript strict + Prisma + BullMQ — type safety end-to-end, Prisma for schema-driven DB, BullMQ for reliable job scheduling. Constrains: no raw SQL, no non-TS files in src/.

## Done

(none yet)
