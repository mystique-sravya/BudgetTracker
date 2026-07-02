# ADR-002 — Adopt Supabase Platform Services

Status: Accepted  
Date: 2026-07-02  
Decision: Use Supabase PostgreSQL, Auth, Storage, Realtime, and Edge Functions as the primary managed platform services.

Alternatives Considered:
- Raw PostgreSQL with separate auth and storage vendors
- Neon
- PlanetScale
- MongoDB-based backend stack

Consequences:
- Platform capabilities stay consistent across auth, storage, and DB layers
- Vendor concentration increases
- RLS and policy design become first-class responsibilities

## Context

FinanceTracker requires relational persistence, authentication, row-level access control, storage, realtime updates, and scheduled jobs without building a custom infrastructure layer.

## Rationale

- Consolidates multiple platform concerns into one managed environment
- Enables RLS-backed tenant isolation
- Reduces operational overhead for a small team
- Fits the modular monolith architecture cleanly