# 22 — Coding Standards

Document Version: 1.0  
Product Name: FinanceTracker  
Last Updated: July 2026

---

## 1. Purpose

Defines coding conventions used across FinanceTracker to keep the codebase readable, consistent, and maintainable.

---

## 2. Naming Conventions

- Components: `PascalCase`
- Hooks: `useCamelCase`
- Variables and functions: `camelCase`
- Types and interfaces: `PascalCase`
- Constants: `UPPER_SNAKE_CASE` only for module-level true constants
- Files: match established conventions in each layer; React components use `PascalCase`, utilities use `camelCase` or `kebab-case` as already established

---

## 3. Folder Conventions

- `app/` for routes, layouts, pages, and Route Handlers
- `components/` for presentational and reusable UI
- `actions/` for Server Actions
- `services/` for business logic
- `repositories/` for persistence access
- `schemas/` for Zod validation contracts
- `types/` for shared type contracts
- `utils/` for stateless helpers
- `emails/` for React Email templates

---

## 4. TypeScript Rules

- Use strict TypeScript mode
- Prefer explicit types at boundaries
- Avoid `any`; use `unknown` and narrow properly
- Share types across layers where contracts are stable
- Keep runtime validation with Zod at request and provider boundaries

---

## 5. Error Handling Rules

- Validate before executing business logic
- Use structured error responses at entry points
- Do not expose raw internal errors to clients
- Capture unexpected failures in Sentry
- Keep fallback behavior explicit for AI and external providers

---

## 6. State Management Rules

- Use Zustand for local UI state only
- Use TanStack Query for server-derived state
- Do not duplicate remote state permanently in local stores
- Revalidate or invalidate after mutations instead of manually synchronizing large state graphs

---

## 7. Database Rules

- All persistence goes through repositories
- Do not write SQL in components, pages, or Route Handlers
- Use transactions for multi-record mutations
- Keep RLS-aligned ownership rules consistent with service-layer authorization

---

## 8. Commit Message Format

Preferred format:

```text
<scope>: <short summary>
```

Examples:

- `docs: refine system architecture`
- `auth: add protected route guard`
- `api: add budget summary endpoint`

---

## 9. Pull Request Checklist

- Scope is clear and limited
- Documentation updated if behavior changed
- Types, lint, and tests pass
- Security and authorization impact reviewed
- New environment variables documented
- Screenshots or examples attached for UI changes when useful

---

## 10. Review Expectations

- Favor simple, explicit code over clever code
- Preserve layer boundaries
- Remove duplication instead of hiding it behind indirection without need
- Prefer root-cause fixes over surface patches

---

**Document Status:** Active