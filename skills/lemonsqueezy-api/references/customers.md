# Customers API

Official section: [Customers](https://docs.lemonsqueezy.com/api/customers).

## Operations

- `POST /v1/customers`: create a customer related to a store.
- `GET /v1/customers/{id}`: retrieve a customer.
- `PATCH /v1/customers/{id}`: update supported customer attributes.
- `PATCH /v1/customers/{id}` with the documented archived status: archive a customer through the update operation.
- `GET /v1/customers`: list/filter customers.

## Implementation rules

- Scope every get/update/archive to the authorized local account and expected store; provider ID possession is not authorization.
- Normalize email for search, but do not use email alone as a durable identity key. Duplicate customers and email changes are possible.
- Use relationships or scoped list filters only when documented. Verify `store_id` after retrieval.
- Customer portal URLs returned in customer/subscription data are opaque, signed, and potentially expiring. Generate/fetch just in time and never log them.
- Archive is a destructive mutation. Explain provider effects, require live confirmation, record actor/reason, and verify state afterward.
- Minimize stored customer PII and define retention/deletion behavior consistent with application policy.

For a customer lookup, join provider customer, orders, subscriptions, invoices, and licenses using IDs/relationships; disclose source and freshness.
