---
name: lemonsqueezy-integration
description: Architect, implement, migrate, debug, or review a complete Lemon Squeezy integration for applications, websites, services, and digital products, including checkout, billing accounts, catalog mappings, subscriptions, entitlements, usage, licenses, customer portal, reconciliation, and go-live operations. Use for cross-cutting integration design; use narrower Lemon Squeezy skills for endpoint, webhook, language, or admin details.
---

# Lemon Squeezy Integration

Treat Lemon Squeezy as the payment/Merchant-of-Record provider and the application as the authority for users, tenants, permissions, features, and auditable entitlement decisions.

## Inspect before designing

1. Read applicable repository instructions and map existing identity, tenancy, plans, billing, persistence, jobs, secrets, HTTP clients, webhooks, observability, and tests.
2. Record current provider integration, test/live IDs, migration constraints, and the exact user journeys requested.
3. Preserve sound architecture and naming. Introduce a provider adapter only where it creates a real boundary.
4. Choose the minimal relevant companion skill:
   - `$lemonsqueezy-api` for direct endpoint contracts and per-resource references.
   - `$lemonsqueezy-webhooks` for inbound synchronization.
   - `$lemonsqueezy-javascript`, `$lemonsqueezy-go`, or `$lemonsqueezy-python` for language-specific implementation.
   - `$lemonsqueezy-admin` for internal operations UI and settings.

## Read by concern

- [references/architecture.md](references/architecture.md): boundaries, local model, source of truth, privacy, and provider isolation.
- [references/lifecycle.md](references/lifecycle.md): checkout through subscription/entitlement/licensing lifecycle.
- [references/operations.md](references/operations.md): reconciliation, observability, migrations, test mode, and go-live.

## Core invariants

- API keys and webhook secrets stay server-side. Payment card collection stays on Lemon Squeezy-hosted surfaces.
- Test and live credentials, IDs, webhooks, records, and metrics are distinct and visibly labeled.
- Store/resource IDs supplied by a client are context, never authorization. Reload local ownership and expected provider mapping on the server.
- Stable local plan keys map to provider store/product/variant/price IDs per mode. Fail closed on unknown mappings; never choose the first API result.
- Provider state and application entitlement are separate fields. Preserve why and until when access is granted.
- Webhooks are the normal update path; scheduled fully paginated reconciliation repairs missed/out-of-order data.
- Money remains integer minor units plus ISO currency. Time remains UTC plus explicit business/reporting time zone.
- Mutations use fresh provider state, server authorization, audit, and safe ambiguity handling. Code work never authorizes a live mutation.

## Deliverable expectations

When implementing, normally include:

- configuration schema and secret-safe environment example;
- provider adapter/service and typed errors;
- local migrations/models for mappings, snapshots, entitlements, webhook inbox, and audit/outbox as needed;
- authorized application use cases/endpoints rather than browser-to-provider secrets;
- webhook receiver and idempotent projection;
- reconciliation/backfill path;
- tests across lifecycle, duplicates/order, mode/store isolation, errors, and access transitions;
- runbook notes for setup, rotation, recovery, and go-live.

Do not create unused infrastructure when the requested feature is narrow. Explain intentionally deferred pieces and their impact.

## Source freshness

References were checked against [Lemon Squeezy API docs](https://docs.lemonsqueezy.com/api) and [developer guides](https://docs.lemonsqueezy.com/guides/developer-guide) on 2026-09-09. Verify drift-prone endpoint fields and SDK signatures live before relying on them.
