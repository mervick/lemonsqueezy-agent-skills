# Reliability, Replay, and Tests

## Inbox state machine

Useful states are received, processing, processed, ignored, and failed, with received/processed times, attempts, next attempt, sanitized error, resource/event metadata, and payload reference. Claim work atomically so two workers cannot process the same record.

Use a database unique constraint for the delivery fingerprint. A check followed by insert is insufficient under concurrency.

## Ordering

- Compare provider `updated_at` or lifecycle timestamps with the applied snapshot.
- No-op older snapshots and retain evidence.
- For equal/ambiguous timestamps or event-specific deltas, fetch current provider state in a reconciliation job.
- Do not assume HTTP arrival order matches business order.

## Acknowledgement and retries

Return 200 after durable acceptance, not after every downstream side effect. If durable storage fails, return non-2xx so the provider retries. Worker retries must be bounded with dead-letter/manual review and idempotent downstream actions.

## Replay

An authorized replay action shows event/resource/mode/store, original receipt, attempts, current local state, and possible effects. It requeues the original verified inbox item and uses normal dedupe/ordering guards. Audit actor, reason, and result.

## Reconciliation

Schedule fully paginated, store/mode-scoped reads for subscriptions/orders/licenses needed by the product. Compare provider snapshots, repair through the same projector, and record scanned/changed/failed/ambiguous counts. Never let a partial page or partial failure appear as a full success.

## Test matrix

- valid raw-body signature
- modified byte, wrong/missing secret/signature, malformed hex, and constant-time compare path
- malformed JSON after valid signature
- header/meta event mismatch and unknown event
- wrong store, wrong mode, missing local mapping, forged custom data
- duplicate sequential/concurrent delivery
- older event after newer event
- database failure before ack and worker failure after ack
- outbox deduplication under retry
- replay of processed and failed deliveries
- reconciliation of a missed event and repeat reconciliation idempotency

## Operational signals

Alert on sustained signature failures, unknown events, webhook silence, processing failure/dead letters, queue age, reconciliation drift, stale provider snapshots, and rate-limit pressure. Correlate sanitized request/inbox/job/audit IDs without logging full payloads.
