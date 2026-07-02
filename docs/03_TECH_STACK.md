# 03 — Tech Stack

**Document Version:** 1.0  
**Product:** FinanceTracker  
**Last Updated:** July 2026

---

## 1. Purpose

Defines the selected technology stack and the reasoning behind each choice. Serves as the canonical Architecture Decision Record (ADR) for the project.

---

## 2. Design Principles

| Principle | Implication |
|-----------|-------------|
| Type safety first | Compile-time errors over runtime surprises — non-negotiable for financial data |
| Integrated > assembled | Prefer platforms that bundle auth, storage, and DB over gluing separate services |
| Managed > self-hosted | Minimize operational surface area; build features, not infrastructure |
| SQL-first | ACID transactions, foreign keys, and relational integrity required |
| Edge-deployed | Sub-100ms response time globally without managing servers |

---

## 3. Technology Overview

| Area | Selected | Rejected Alternatives |
|------|-----------|-----------------------|
| Framework | Next.js 15 (App Router) | React + Vite, Remix, SvelteKit |
| Language | TypeScript 5+ (strict) | JavaScript |
| Styling | Tailwind CSS v3 | CSS Modules, Styled Components |
| Components | shadcn/ui (Radix UI) | MUI, Chakra UI |
| Icons | Lucide React | Heroicons, Font Awesome |
| UI State | Zustand | Redux, Context API |
| Server State | TanStack Query v5 | SWR, Apollo |
| Forms + Validation | React Hook Form + Zod | Formik + Yup |
| Charts | Recharts | ApexCharts, Chart.js, D3.js |
| Animations | Framer Motion | React Spring, GSAP |
| Date Utilities | date-fns | Moment.js, Luxon |
| Backend | Next.js Route Handlers + Server Actions | Express, Fastify, tRPC |
| Database | Supabase (PostgreSQL 15+) | Neon, PlanetScale, MongoDB |
| ORM | Drizzle | Prisma, Kysely, TypeORM |
| Authentication | Supabase Auth | NextAuth.js, Clerk, Auth0 |
| AI | Google Gemini Flash 2.0 | GPT-4o, Claude 3.5, local LLMs |
| Email Delivery | Resend | SendGrid, AWS SES |
| Email Templating | React Email | Raw HTML templates, MJML |
| In-App Notifications | Sonner | React Hot Toast |
| File Storage | Supabase Storage | AWS S3, Cloudflare R2 |
| Scheduled Jobs | Supabase Edge Functions | Bull + Redis, Inngest |
| Unit Testing | Vitest | Jest |
| E2E Testing | Playwright | Cypress |
| Deployment | Vercel | Railway, Render, self-hosted |
| Error Monitoring | Sentry | LogRocket, Datadog |
| CI/CD | GitHub Actions | GitLab CI |
| Linting / Formatting | ESLint + Prettier + Husky | Biome, Standard |

---

## 4. Architecture Overview

```
┌─────────────────────────────────────────┐
│         Browser (Next.js App Router)    │
│  Zustand · TanStack Query · Framer Motion│
└───────────────────┬─────────────────────┘
                    │ HTTPS
┌───────────────────▼─────────────────────┐
│        Vercel Edge Network              │
│  Route Handlers · Server Actions        │
│  Middleware (auth guard, rate limit)    │
└───────────────────┬─────────────────────┘
                    │
┌───────────────────▼─────────────────────┐
│               Supabase                  │
│  PostgreSQL ── Drizzle ORM              │
│  Auth (JWT + RLS)                       │
│  Storage (CDN)                          │
│  Realtime (websocket broadcasts)        │
│  Edge Functions (cron jobs)             │
└───────┬──────────────────┬──────────────┘
        │                  │
┌───────▼──────┐  ┌────────▼──────────────┐
│ Gemini Flash │  │  Resend · Sentry      │
│  (AI/ML)     │  │  (Email · Monitoring) │
└──────────────┘  └───────────────────────┘
```

No separate backend. No standalone auth server. No WebSocket infra. No job queue infrastructure.

---

## 5. Frontend Stack

### Next.js 15 — App Router

Full-stack framework. Server Components reduce JS bundle. Server Actions eliminate API boilerplate for mutations. File-based routing with layout nesting for protected routes.

**Tradeoffs:**
- Vendor affinity with Vercel is real but acceptable given zero-config deployment
- Server Components and Server Actions have a learning curve; documented in CLAUDE.md

---

### TypeScript 5+ — Strict Mode

Strict mode enabled globally. `Amount` types always `number` (cents, never floats). Shared types live in `/types` — no duplication between client and server.


---

### Tailwind CSS v3

Utility-first. JIT compiler — only used classes ship. Custom tokens defined in `tailwind.config.ts`:

**Why not CSS Modules:** Constant file switching for co-located styles slows iteration.  
**Why not Styled Components:** Runtime CSS injection has measurable overhead on dashboard renders.

---

### shadcn/ui + Radix UI

Components are copied into `/components/ui` — not an npm dependency. Full ownership. Radix provides accessible primitives (focus management, ARIA, keyboard nav). Tailwind-styled.

**Why not MUI:** Bundles its entire component library. Theme customization is fighting a system.  
**Why not Chakra UI:** Runtime styling engine adds unnecessary overhead.

---

### State Management

**Zustand** — UI state (sidebar open, active filters, modal state). 5-line setup. No provider tree. Persists to localStorage with `persist` middleware where needed.

**TanStack Query v5** — Server state. Handles caching, background refetching, optimistic updates, and pagination. Transactions list, budget totals, and AI responses all flow through Query.

---

### Forms — React Hook Form + Zod

Hook Form uses refs (uncontrolled) — no re-render per keystroke. Zod schemas drive both runtime validation and TypeScript types via `z.infer<>`. Same schema reused on the server.

---

### Charts + Animations

**Recharts** — React-first. Composable (`<AreaChart>` + `<Tooltip>` + `<Legend>`). Responsive via `<ResponsiveContainer>`. Used for spending trends, budget utilization bars, category breakdowns.

**Framer Motion** — Layout animations for list reordering, streak celebration sequences, page transitions. `layoutId` prop drives shared-element transitions.

**date-fns** — Tree-shakeable date utilities. Used throughout for period calculations, streak logic, and report date ranges.

---

## 6. Backend Stack

### Next.js Route Handlers + Server Actions

Route Handlers (`app/api/`) expose REST endpoints for external consumption or complex AI workflows. Server Actions handle all form mutations directly — no serialization overhead.

Middleware at `middleware.ts` handles JWT verification on every protected route, request logging, and rate limiting (10 req/s per user on AI endpoints).

---

## 7. Database Stack

### Supabase PostgreSQL 15+

**Why PostgreSQL:** ACID-compliant. Foreign key constraints enforce relational integrity. Partial indexes for performance on filtered queries. JSON columns for flexible AI metadata.

**Why Supabase over raw Postgres:** Auth, RLS, Realtime, Storage, and Edge Functions integrated — eliminates 4–5 separate service dependencies.

**Row Level Security (RLS)** enforces data isolation at the database layer:


**Why not PlanetScale:** No foreign keys — dealbreaker for financial data integrity.  
**Why not MongoDB:** Wrong tool; requires ACID and relational integrity.

---

### Drizzle ORM

Schema-as-code. TypeScript types derived directly from schema — no generation step. SQL-like query builder with full type inference. Lightweight (no Prisma engine binary).



**Why not Prisma:** Heavy engine binary. Slower cold starts. Less direct SQL control.

---

## 8. Authentication

### Supabase Auth

Email/password with verification. Magic links. OAuth-ready (Google, GitHub). JWTs issued and refreshed automatically. HttpOnly cookies prevent XSS token theft. PKCE flow for OAuth.

**Session flow:**
1. User signs in → Supabase issues JWT (access + refresh)
2. Supabase client sets HttpOnly cookie automatically
3. Next.js middleware validates JWT on every request
4. `auth.uid()` available in all RLS policies — no app-layer filtering needed

**Why not NextAuth:** Adds dependency for functionality Supabase already provides.  
**Why not Clerk:** External vendor adds cost and data dependency.  
**Why not custom:** Auth is too security-critical to build in-house.

---

## 9. AI Stack

### Google Gemini Flash 2.0

**Use cases:**
- Weekly spend summaries with natural language narrative
- Budget overage alerts with actionable suggestions
- Anomaly detection (unusual merchant, category spike)
- Categorization suggestions for uncategorized transactions

**Why Gemini Flash:**

| Factor | Gemini Flash 2.0 | GPT-4o |
|--------|-----------------|--------|
| Free tier | 15 req/min, 1.5M tokens/day | None |
| Context window | 1M tokens | 128K tokens |
| Cost per 1M tokens | $0.075 | $2.50 |
| JSON output mode | Native | Supported |

AI responses cached in Supabase (`ai_insights` table) with 24-hour TTL to avoid redundant API calls. Zod validates every structured response before use.

---

## 10. Infrastructure

### Vercel

Zero-config deployment. Git push to `main` → production. Every PR gets a preview URL. Edge network serves static assets and API from 100+ PoPs. No cold starts on hobby plan.

### Supabase Edge Functions

Deno-based serverless functions handle all scheduled work:
- Daily budget threshold checks → Resend email notifications
- Weekly AI summary generation and dispatch
- Monthly rollup aggregates for reporting

### Resend + React Email

Resend handles email delivery. React Email is the templating layer, letting the team author and maintain transactional emails as React components instead of raw HTML. This aligns with the existing React/TypeScript stack and keeps password reset, budget alert, and weekly summary templates easier to evolve safely.

### Sentry

Automatic exception capture. Source maps uploaded on deploy — stack traces show TypeScript, not minified output. 5K errors/month on free tier.

---

## 11. Development Tooling

| Tool | Role |
|------|------|
| ESLint | Catch bugs and enforce consistency at lint time |
| Prettier | Autoformat on save; no style debates |
| Husky | Pre-commit: lint + type-check; pre-push: unit tests |
| Vitest | Unit and integration tests; 10× faster than Jest; native ESM |
| Playwright | E2E tests; cross-browser; auto-wait; codegen for recording |
| GitHub Actions | CI: lint → type-check → test → deploy preview |
| Supabase CLI | Local DB with `supabase start`; migration management |

**Version pins:**

| Package | Version |
|---------|---------|
| next | 15.x |
| react | 18.x |
| typescript | 5.x |
| tailwindcss | 3.x |
| drizzle-orm | latest |
| @supabase/supabase-js | latest |
| node (dev) | 20.x LTS |

---

## 12. Risks & Tradeoffs

| Risk | Severity | Mitigation |
|------|----------|------------|
| Supabase vendor lock-in | Medium | Drizzle schema is portable; migrations are plain SQL |
| Gemini API quota limits | Low | 24-hour response cache; graceful degradation if AI unavailable |
| Vercel cold starts | Low | Edge functions have no cold starts; serverless <50ms |
| Server Component complexity | Medium | Documented patterns in CLAUDE.md; clear client/server boundaries |
| Drizzle migration conflicts | Low | Migrations reviewed in CI before merge; single-writer pattern |
| Gemini output hallucinations | Medium | Zod schema validates every AI JSON response before use |
| Tailwind class sprawl | Low | Component extraction enforced at >3 reuses; `cn()` for conditionals |

---

## 13. Future Evolution

| Trigger | Evolution |
|---------|-----------|
| Mobile app needed | Add Expo (React Native) — same Supabase backend, shared Zod types |
| Receipt OCR | Gemini multimodal already supports image input — no new model |
| Real-time collaboration | Supabase Realtime already integrated — enable per-budget subscriptions |
| International users | `date-fns` locale support + `Intl` API for currency formatting |
| AI cost exceeds budget | Switch to Gemini Nano (on-device) or fine-tune a smaller open model |
| Traffic scale-up | Vercel auto-scales; Supabase connection pooling via PgBouncer |
| Audit trail required | Enable `pg_audit` extension — no schema changes needed |

---

## 14. Stack Summary

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 15 (App Router) |
| Language | TypeScript 5+ strict |
| Styling | Tailwind CSS v3 |
| Components | shadcn/ui + Radix UI |
| Icons | Lucide React |
| UI State | Zustand |
| Server State | TanStack Query v5 |
| Forms + Validation | React Hook Form + Zod |
| Charts | Recharts |
| Animations | Framer Motion |
| Date Utilities | date-fns |
| Backend API | Next.js Route Handlers |
| Mutations | Next.js Server Actions |
| Database | Supabase PostgreSQL 15+ |
| ORM | Drizzle |
| Auth | Supabase Auth (JWT + RLS) |
| AI | Google Gemini Flash 2.0 |
| Email Delivery | Resend |
| Email Templating | React Email |
| In-App Toasts | Sonner |
| File Storage | Supabase Storage |
| Scheduled Jobs | Supabase Edge Functions |
| Unit Tests | Vitest |
| E2E Tests | Playwright |
| Deployment | Vercel |
| Error Monitoring | Sentry |
| CI/CD | GitHub Actions |
| Linting | ESLint + Prettier + Husky |

---

**Next:** [04_SYSTEM_ARCHITECTURE.md](04_SYSTEM_ARCHITECTURE.md)
