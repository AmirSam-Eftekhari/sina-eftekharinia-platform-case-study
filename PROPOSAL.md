# Engineering Proposal (reconstructed)

> This proposal was reconstructed **after** implementation from the project's phase briefs and the code. It is not a pre-implementation document, and its requirements are marked with what the code shows.

## Context and problem
A working violinist needs an online presence in English and Persian. Updating a static site requires a developer and a redeploy; the owner wants to edit content, publish concerts and sell merchandise listings from a phone.

## Objectives
1. Professional bilingual website with correct SEO and localization.
2. Owner-editable content without redeploys, with a draft/publish step.
3. Tickets and Shop sections that are data-driven.
4. A mobile admin app without passwords or embedded secrets.
5. Operational visibility (health checks) and honest failure behavior.

## Users and use cases
Visitors (read in either language, find concerts, contact). One administrator (edit text and photos, publish concerts/products, check site health, manage devices).

## Scope
In: website, admin API, Android-first admin app, Postgres/Storage schema. Out (this phase): payment processing, order management, internal ticket sales.

## Requirements
**Functional** (all observed in code): localized routes; editable pages and lists in both languages; draft/publish per language; preview; events and products with inventory and discounts; shop modes including hidden; image upload; translation suggestions via provider adapter; device activation and revocation; health checks.

**Non-functional:** no secrets in the app; public roles cannot read drafts or write; uploads validated and sanitized; build must not publish empty content when the database is down; schema must allow payments later without renames. Performance, availability and accessibility targets were **not specified or measured**.

## Architecture
See [ARCHITECTURE.md](ARCHITECTURE.md).

## Security and privacy requirements
See [SECURITY-AND-PRIVACY.md](SECURITY-AND-PRIVACY.md).

## Deployment
Vercel + Supabase; EAS for Android. See [docs/deployment-overview.md](docs/deployment-overview.md).

## Acceptance criteria and evidence
Automated tests, type-checks, production builds: see [docs/testing-and-validation.md](docs/testing-and-validation.md). Device testing and live-deployment checks are **unverified**.

## Limitations and future work
Variants UI, payments, audio/video upload, drag-and-drop ordering, localized admin UI, garbage collection of images uploaded but never saved, full TLS certificate verification for the database connection by default.
