# ADR-003 — Adopt Drizzle ORM

Status: Accepted  
Date: 2026-07-02  
Decision: Use Drizzle ORM for schema-aware query construction and transaction handling.

Alternatives Considered:
- Prisma
- Kysely
- TypeORM

Consequences:
- Repositories remain explicit and query-oriented
- More SQL awareness is required from contributors
- Code generation overhead stays low

## Context

FinanceTracker needs typed persistence access while keeping SQL control visible and repository implementations lightweight.

## Rationale

- Preserves SQL-shaped control in repositories
- Provides strong TypeScript support without heavy runtime tooling
- Fits a layered repository architecture well