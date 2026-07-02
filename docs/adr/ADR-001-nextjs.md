# ADR-001 — Adopt Next.js App Router

Status: Accepted  
Date: 2026-07-02  
Decision: Use Next.js 15 App Router as the primary application framework.

Alternatives Considered:
- React + Vite
- Remix
- SvelteKit

Consequences:
- The app remains a single full-stack web deployable
- Team conventions must distinguish Server Components and Client Components clearly
- Framework coupling increases, but operational simplicity improves

## Context

FinanceTracker requires one web runtime that supports server rendering, protected routes, server-side mutations, and a low-overhead deployment model.

## Rationale

- Supports server-first rendering for dashboard-heavy pages
- Provides Route Handlers and Server Actions in one runtime
- Simplifies route composition and layout boundaries
- Aligns cleanly with Vercel deployment