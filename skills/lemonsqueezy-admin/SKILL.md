---
name: lemonsqueezy-admin
description: Design, implement, or review an internal Lemon Squeezy billing admin panel and settings for customers, orders, subscriptions, refunds, discounts, licenses, catalog mappings, webhooks, analytics, audit, and test/live connection health. Use for operator UI and backend administration in any application, website, service, or digital product.
---

# Lemon Squeezy Admin Panel

Build an internal operations surface that is safe around real money, customer access, multiple tenants/stores, test/live modes, stale data, and provider failures.

## Workflow

1. Inspect existing navigation/design system, identity/roles, tenancy, billing service, database, jobs, audit, and tests.
2. Identify actual operator roles and tasks. Do not clone Lemon Squeezy's own dashboard or expose actions the backend cannot support.
3. Define each page's source of truth, freshness, scope, filters, pagination, permissions, failure states, mutation review, and audit.
4. Keep provider calls server-side and use `$lemonsqueezy-api`, the relevant language skill, and `$lemonsqueezy-webhooks` for underlying implementation.

Read [references/pages.md](references/pages.md) for navigation/page contracts, [references/settings.md](references/settings.md) for all integration settings, and [references/actions-and-reporting.md](references/actions-and-reporting.md) for mutations, support, dashboards, and exports.

## Invariants

- Show active test/live mode and store on every page; never silently mix them.
- Enforce role, tenant, store, and resource ownership on the server. Hidden controls are not authorization.
- Use local synchronized read models for fast tables/reports when available; obtain fresh provider state before material actions.
- Show last sync, source, stale/partial state, filters, reporting time zone, and currency.
- Use integer minor units. Group different currencies or use an explicit documented conversion; never silently sum them.
- Mask PII, license keys, signed URLs, payment metadata, and secret fingerprints. Never return API key/webhook secret values.
- Add accessible labels/status text, keyboard/focus behavior, validation, loading/empty/error/stale/partial states, and responsive layout.
- Financial/access mutations require a consequence review, double-submit protection, server revalidation, reason/audit, and post-action verification.
- Building/testing an admin feature does not authorize any live provider mutation.

## Definition of done

- UI and backend cover the requested operator journey, including failure and permission states.
- Cross-tenant/store/mode access is denied server-side and tested.
- Critical actions cannot run against stale resources or be duplicated accidentally.
- Metrics are complete for their stated scope and disclose exclusions.
- Tests cover accessibility, permission/mode labeling, confirmation, stale/partial data, provider errors, audit, and backend ownership.

References were checked against current Lemon Squeezy documentation on 2026-09-09.
