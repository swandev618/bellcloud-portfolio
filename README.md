# BellCloud — Portfolio Case Study

A management platform for portable industrial gas detectors (**BellLite** / **BellMini**) —
live fleet monitoring, on-device binary log retrieval over Bluetooth and a serial cradle, and
browser-direct device configuration with no server in the data path.

**📄 [Read the case study →](https://swandev618.github.io/bellcloud-portfolio/)**

---

## What this repository is

BellCloud is commercial software and its source repository is private. This repository holds a
written case study of the architecture and the engineering decisions behind it — what the hard
problems were, which trade-offs I chose, and what remains unresolved.

No source code, hostnames, credentials, or customer data are included here.

## The project

| | |
|---|---|
| **Role** | Sole developer — frontend, backend, database, protocol layer, CI/CD, operations |
| **Duration** | June – November 2026 (5 months) |
| **Codebase** | ~110,000 LOC TypeScript across a Yarn 4 workspaces monorepo |
| **Scale** | 39 database models · 127 API routes · 91 protocol function definitions · 3 locales × 997 keys · 130 unit tests |

### Stack

**Frontend** — React 19, Vite 7, TypeScript, Zustand, TanStack Query, Emotion, ECharts,
Web Bluetooth, Web Serial, PWA (custom service worker), react-i18next

**Backend** — NestJS 11, Prisma, PostgreSQL, Redis, Firebase Admin (FCM), Passport/JWT,
`@nestjs/schedule`, Winston

**Infrastructure** — Docker Compose, GitHub Actions, GHCR, AWS EC2 + RDS, Nginx

## Topics covered in the case study

1. **The product** — four screens from the running production system, each linked to the section that explains the engineering behind it
2. **Architecture** — why live telemetry and historical log retrieval are kept as two separate data paths
3. **Device protocol** — packet layouts declared as data tables instead of hand-written encoders; a shared CRC-16 implementation; the UTC epoch constant that three independent code paths depend on
4. **Browser ↔ device** — one adapter interface over Web Bluetooth and Web Serial, and three bugs that only appear against real hardware
5. **Log ingestion** — a three-stage pipeline that preserves raw bytes so a parser fix never requires re-downloading from the device; database-enforced idempotency; one hot table optimised four ways
6. **Live aggregation** — three freshness tiers with client poll intervals paired to server cache TTLs; bitwise alarm counting in raw SQL without double-counting packets
7. **Alarm & push** — edge-triggered notifications, and device-offline detection built on Redis key expiry rather than a polling sweep
8. **Auth & tenancy** — a five-level role hierarchy as a numeric comparison, a fallback that had to fail closed, and instant revocation of already-issued access tokens
9. **Operations** — production and staging co-resident on one host, migrations applied twice on purpose, and a deployed log-inspection CLI
10. **Post-mortem** — two bugs behind one incident, and why a configuration edit made on a server is not a fix
11. **Scope & limits** — what is unresolved, stated plainly

## Screenshots

The four captures in `screens/` come from the live production system, from our own tenant.
Personal names, email addresses and phone numbers are replaced with placeholders; device counts,
serials, calibration figures and sensor readings are real.

## A note on accuracy

Every figure in the case study is measured from the codebase, not estimated. The "Scope & Limits"
section exists because a case study that lists only successes isn't useful for evaluating
engineering judgement — unresolved issues (BLE burst transfer, test coverage gaps, absent
partitioning) are named there directly.

---

**Contact** — [github.com/swandev618](https://github.com/swandev618)
