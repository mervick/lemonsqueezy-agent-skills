---
name: lemonsqueezy-webhooks
description: Implement, debug, secure, test, or operate Lemon Squeezy webhook receiving, signature verification, durable inboxes, idempotent event processing, entitlement synchronization, replay, and reconciliation. Use whenever application state depends on Lemon Squeezy events.
---

# Lemon Squeezy Webhooks

Build for duplicate, delayed, missing, concurrent, and out-of-order delivery. Receiving valid JSON is not sufficient evidence that processing is reliable.

## Secure receive sequence

1. Accept POST over public HTTPS with a strict method/content/body-size policy.
2. Read the exact raw bytes once. Do not parse, normalize, decompress, or reserialize first.
3. Compute lowercase hex HMAC-SHA256 using the mode/store-specific signing secret.
4. Compare with `X-Signature` in constant time. Reject before parsing or side effects.
5. Parse JSON and cross-check `X-Event-Name` with `meta.event_name` when both exist.
6. Validate expected store and `test_mode`; custom data cannot select an arbitrary tenant.
7. Insert a durable inbox record under a database uniqueness constraint.
8. Return HTTP 200 promptly after durable acceptance and process asynchronously when possible.

Read [references/events.md](references/events.md) for event-family projection and [references/reliability.md](references/reliability.md) for idempotency, ordering, replay, reconciliation, tests, and operations.

## Invariants

- Lemon Squeezy retries failed delivery up to three more times; duplicate handling is mandatory.
- If no delivery ID exists, derive a stable fingerprint from verified raw body plus event name and retain resource ID/provider timestamps for ordering.
- Unknown events are recorded/ignored safely; they never grant or revoke access.
- Older provider snapshots do not overwrite newer applied state.
- Downstream email/provisioning jobs use an outbox or equivalent dedupe boundary.
- Logs redact PII, secrets, license keys, and signed URLs; full payload retention requires explicit privacy/retention policy.
- Replay uses the normal idempotent handler and never bypasses original verification/store/mode evidence.

## Mutation boundary

Creating/updating/deleting a live webhook registration is an external mutation. Show exact mode, store, URL, events, and rotation effect and obtain explicit user confirmation before executing it.

## Primary source

Use the current [webhook guide](https://docs.lemonsqueezy.com/guides/developer-guide/webhooks) and [Webhooks API](https://docs.lemonsqueezy.com/api/webhooks). These references were checked on 2026-09-09.
