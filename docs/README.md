# FinanceTracker Documentation

Welcome to the FinanceTracker technical documentation.

This documentation covers the product vision, architecture, system design, database, APIs, security, AI integration, deployment, and engineering standards used throughout the project.

The documents are intended for developers, contributors, reviewers, and future maintainers.

## Document Map

## Product

1. `01_PRODUCT_OVERVIEW.md`
2. `02_REQUIREMENTS.md`
3. `20_ROADMAP.md`

## Architecture

1. `03_TECH_STACK.md`
2. `04_SYSTEM_ARCHITECTURE.md`
3. `10_FRONTEND_ARCHITECTURE.md`
4. `11_BACKEND_ARCHITECTURE.md`
5. `15_AI_ARCHITECTURE.md`
6. `19_DEPLOYMENT.md`

## Database

1. `05_DATABASE_DESIGN.md`
2. `06_DATABASE_SCHEMA.md`

## Security

1. `07_AUTHENTICATION.md`
2. `08_AUTHORIZATION.md`
3. `17_SECURITY.md`

## Development

1. `09_API_DESIGN.md`
2. `12_UI_UX.md`
3. `13_COMPONENT_LIBRARY.md`
4. `14_FEATURES.md`
5. `16_NOTIFICATION_SYSTEM.md`
6. `18_TESTING.md`
7. `21_CONTRIBUTING.md`
8. `22_CODING_STANDARDS.md`

## Document Status

| Document | Status |
|----------|--------|
| Product Overview | Complete |
| Requirements | Complete |
| Tech Stack | Complete |
| System Architecture | Complete |
| Database Design | Draft |
| Database Schema | Draft |
| Authentication | Draft |
| Authorization | Draft |
| API Design | Draft |
| Frontend Architecture | Draft |
| Backend Architecture | Draft |
| UI/UX | Draft |
| Component Library | Draft |
| Features | Draft |
| AI Architecture | Draft |
| Notification System | Draft |
| Security | Draft |
| Testing | Draft |
| Deployment | Draft |
| Roadmap | Draft |
| Contributing | Draft |
| Coding Standards | Active |

## Supporting Folders

- `adr/` — architecture decision records for major platform choices
- `diagrams/` — editable Mermaid source files for system diagrams
- `assets/` — static documentation assets such as images and exported diagrams

## Quick Navigation

| If you want to... | Read |
|-------------------|------|
| Understand the product | `01_PRODUCT_OVERVIEW.md`, `02_REQUIREMENTS.md` |
| Learn the architecture | `03_TECH_STACK.md`, `04_SYSTEM_ARCHITECTURE.md` |
| Understand the database | `05_DATABASE_DESIGN.md`, `06_DATABASE_SCHEMA.md` |
| Understand authentication and authorization | `07_AUTHENTICATION.md`, `08_AUTHORIZATION.md` |
| Build APIs | `09_API_DESIGN.md` |
| Understand frontend structure | `10_FRONTEND_ARCHITECTURE.md`, `12_UI_UX.md`, `13_COMPONENT_LIBRARY.md` |
| Understand backend structure | `11_BACKEND_ARCHITECTURE.md` |
| Learn AI behavior | `15_AI_ARCHITECTURE.md` |
| Learn security controls | `17_SECURITY.md` |
| Understand testing strategy | `18_TESTING.md` |
| Deploy the application | `19_DEPLOYMENT.md` |

## Suggested Reading Order

1. Product overview
2. Requirements
3. Tech stack
4. System architecture
5. Database design and schema
6. Authentication and authorization
7. API, frontend, and backend architecture
8. AI, notifications, security, testing, and deployment

## Notes

- The root `README.md` is the repository landing page.
- This `docs/README.md` is the documentation index.
- ADRs capture why major technology or architecture choices were accepted.
- Mermaid sources should be updated alongside architecture or flow changes.

## Documentation Principles

- Keep documentation close to implementation.
- Update documentation with every architectural change.
- Prefer diagrams over long paragraphs where appropriate.
- Keep Mermaid diagrams synchronized with implementation.
- Record significant architectural decisions as ADRs.

## Diagram Sources

Primary editable Mermaid sources live in `docs/diagrams/`.

- `backend/system.mmd` — system architecture overview
- `backend/domains.mmd` — domain relationships
- `backend/request-flow.mmd` — end-to-end request runtime
- `auth/authentication.mmd` — auth flow
- `auth/access-control.mmd` — authorization flow
- `database/database.mmd` — persistence interaction flow
- `ai/ai.mmd` — AI pipeline
- `backend/notifications.mmd` — notification pipeline
- `deployment/deployment.mmd` — CI/CD and deployment path

Additional `.mmd` files in `auth/`, `backend/`, `frontend/`, and `database/` cover client, dependency, storage, error, security, and realtime views.

## Documentation Guidelines

When adding new features:

- Update affected architecture diagrams.
- Update relevant API documentation.
- Add or update ADRs when architectural decisions change.
- Keep diagrams and Markdown synchronized.

## Documentation Layout

```text
docs/
├── 01_PRODUCT_OVERVIEW.md
├── 02_REQUIREMENTS.md
├── 03_TECH_STACK.md
├── 04_SYSTEM_ARCHITECTURE.md
├── 05_DATABASE_DESIGN.md
├── 06_DATABASE_SCHEMA.md
├── 07_AUTHENTICATION.md
├── 08_AUTHORIZATION.md
├── 09_API_DESIGN.md
├── 10_FRONTEND_ARCHITECTURE.md
├── 11_BACKEND_ARCHITECTURE.md
├── 12_UI_UX.md
├── 13_COMPONENT_LIBRARY.md
├── 14_FEATURES.md
├── 15_AI_ARCHITECTURE.md
├── 16_NOTIFICATION_SYSTEM.md
├── 17_SECURITY.md
├── 18_TESTING.md
├── 19_DEPLOYMENT.md
├── 20_ROADMAP.md
├── 21_CONTRIBUTING.md
├── 22_CODING_STANDARDS.md
├── adr/
├── assets/
└── diagrams/
	├── ai/
	├── auth/
	├── backend/
	├── database/
	├── deployment/
	└── frontend/
```

## Documentation Version

| Property | Value |
|----------|-------|
| Product | FinanceTracker |
| Documentation Version | 1.0 |
| Last Updated | July 2026 |
| Maintainer | Project Team |

---

**Document Status:** Active
