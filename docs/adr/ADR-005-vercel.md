# ADR-005 — Adopt Vercel for Application Hosting

Status: Accepted  
Date: 2026-07-02  
Decision: Use Vercel as the primary application hosting platform.

Alternatives Considered:
- Railway
- Render
- Self-managed hosting

Consequences:
- Hosting is closely aligned with framework conventions
- Platform affinity increases
- Deployment flow stays simple for a small team

## Context

FinanceTracker requires preview deployments, managed hosting for Next.js, simple release flow, and low operations overhead.

## Rationale

- Native fit for Next.js App Router deployment
- Preview environments support documentation-driven and UI-heavy review workflows
- Minimizes infrastructure setup for the web runtime