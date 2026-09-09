# Subscription Items and Usage Records APIs

Official sections: [Subscription Items](https://docs.lemonsqueezy.com/api/subscription-items) and [Usage Records](https://docs.lemonsqueezy.com/api/usage-records).

## Operations

- retrieve/list subscription items
- retrieve a subscription item's current usage
- update documented subscription-item attributes
- create, retrieve, and list usage records

## Meter design

- Map one stable local meter to the correct subscription item, account, store, and mode.
- Define units, integer quantity rules, provider aggregation behavior, billing period boundary, corrections, and late-arrival policy.
- Keep a durable local usage ledger/outbox. Do not submit directly from volatile counters or browser requests.
- Connect every provider submission to local ledger IDs and an audit/correlation record.
- Validate ownership and lifecycle before submission; do not let the client choose arbitrary subscription-item IDs.

Usage creation accepts a positive integer `quantity` and an `action`: `increment` adds to current-period usage and belongs with the Sum aggregation; `set` replaces current-period usage and belongs with the Most recent aggregation modes. The documented default is `increment`. Maximum-usage aggregation needs current-guide review rather than a guessed action. A timeout after submission is ambiguous; retrieve current usage/records and reconcile before retrying to avoid double billing.

For high-volume usage, batch locally according to provider semantics and rate limits, but retain enough granularity to audit and correct the aggregate.
