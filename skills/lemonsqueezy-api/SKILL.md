---
name: lemonsqueezy-api
description: Implement, debug, or review direct Lemon Squeezy REST API work across users, stores, customers, catalog, orders, subscriptions, usage billing, discounts, licenses, checkouts, webhooks, and affiliates. Use when endpoint contracts, JSON:API payloads, filters, pagination, or provider errors matter; use a language-specific Lemon Squeezy skill for SDK details.
---

# Lemon Squeezy API

Use the current official API reference as authority and these files as a decision-oriented map. Read only the resource references needed for the task.

## Workflow

1. Inspect the repository's current billing client, store/mode configuration, schemas, and tests.
2. Identify the exact resource and operation. Open its current official endpoint page before coding a mutation or relying on a filter/field.
3. Preserve the project's sound HTTP abstraction; do not introduce a second client merely because an example uses one.
4. Scope provider IDs to the authorized local account, expected store, and test/live mode on the server.
5. Implement typed request/response/error mapping, bounded timeouts, pagination where required, redaction, and tests.
6. Do not execute live API mutations while writing or testing code. Any actual live mutation requires explicit confirmation of exact mode, store, resource, and effect.

## Shared contract

Read [references/protocol.md](references/protocol.md) for authentication, JSON:API, pagination, filters, money, errors, rate limiting, and the separate License API contract.

## Resource routing

- [Users and stores](references/users-stores.md)
- [Customers](references/customers.md)
- [Products, variants, prices, and files](references/catalog.md)
- [Orders and order items](references/orders.md)
- [Subscriptions and subscription invoices](references/subscriptions.md)
- [Subscription items and usage records](references/usage-billing.md)
- [Discounts and discount redemptions](references/discounts.md)
- [License keys, instances, and License API](references/licensing.md)
- [Checkouts](references/checkouts.md)
- [Webhook registrations](references/webhooks-api.md)
- [Affiliates](references/affiliates.md)

## Invariants

- Keep API keys server-side and separate per mode. Never expose them through browser bundles, public environment variables, logs, fixtures, or API errors.
- Treat IDs as opaque strings in domain/storage code. Never use a client-provided provider ID as authorization.
- Store money as integer minor units with its ISO currency. Do not aggregate different currencies without explicit conversion.
- Follow all list pages for complete results. Never label a one-page sample `all time`.
- Do not invent filter, include, sort, status, or mutation support. Check the endpoint page or installed SDK types.
- Additive response fields and webhook event types are backward-compatible changes; parsers must tolerate unknown fields and safely handle unknown events.
- Never blindly retry a refund, usage submission, plan change, discount mutation, or other ambiguous non-idempotent request.

## Source freshness

These references were checked on 2026-09-09. The current main API documentation states 300 calls per minute; the License API separately states 60 calls per minute. Prefer [official API docs](https://docs.lemonsqueezy.com/api) over copied examples whenever they differ.
