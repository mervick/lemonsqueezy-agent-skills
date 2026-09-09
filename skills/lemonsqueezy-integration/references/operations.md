# Operations, Migration, and Go-Live

## Configuration

Maintain distinct test/live values for API key reference, webhook secret, store, product/variant/price mappings, URLs, and observability. Validate connection using authenticated user/store reads and retain only masked key fingerprint, expiry/rotation owner, last success, and sanitized error.

## Webhook-first plus reconciliation

Persist verified deliveries before acknowledgement and project them idempotently. Run scheduled fully paginated reconciliation by store/mode to repair missed events and detect drift. Reuse the same guarded projection logic and prevent older observations from overwriting newer state.

Track queue age, attempts/dead letters, unknown events, signature failures, last successful webhook, last reconciliation, records scanned/changed/failed, provider latency/errors, rate-limit headroom, and entitlement drift.

## Migration/backfill

Use staged migration:

1. introduce nullable provider mappings/snapshots and instrumentation;
2. audit current provider/local identity matches;
3. dry-run a fully paginated backfill and report ambiguous/unmatched rows;
4. review mappings instead of assigning uncertain data to an arbitrary account;
5. apply idempotent backfill with checkpoints;
6. validate counts, money by currency, states, and entitlements;
7. enable webhook projection/read path, then enforce constraints;
8. retain rollback/repair path until live evidence is stable.

## Test strategy

- unit: mappings, money, entitlement policy, HMAC, transitions, redaction
- adapter contract: exact requests, pagination, errors, timeouts, 429, ambiguous mutations
- integration: checkout to signed webhook to entitlement; duplicates/concurrency/reordering; cancellation/resume/failure/recovery/refund; cross-store/mode isolation
- reconciliation: missed webhook, stale snapshot, repair and repeat idempotency
- security: browser bundle/env exposure, authorization, CSRF for admin actions, signed URL/PII masking

Use Lemon Squeezy test-mode credentials and simulated webhook events where available. Mock/test-mode success does not prove live deployment.

## Go-live evidence

- production secrets are in the secret manager and absent from client bundles/logs
- live mappings and registrations are verified against the intended store
- public HTTPS webhook reaches the deployed artifact and a live-mode signed delivery is accepted/processed
- workers, dead-letter/replay, reconciliation, alerts, and dashboards are operating
- key expiry/rotation and webhook-secret rotation have owners/runbooks
- application behavior under provider outage/rate limiting is exercised
- live mutations remain role-gated, reviewed, audited, and verifiable

Report partial or unverified gates honestly.
