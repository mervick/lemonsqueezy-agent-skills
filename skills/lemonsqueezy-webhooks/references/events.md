# Event Projection

Select only events the application handles and verify the current official list/schema before implementation. Common families include:

- orders: `order_created`, `order_refunded`
- customers: `customer_updated`
- subscription lifecycle: created, updated, cancelled, resumed, expired, paused, unpaused
- subscription payments: success, failed, recovered
- licenses: created, updated
- affiliates when used by the application

Lemon Squeezy considers new event types backward-compatible additions. Unknown events must not crash the receiver or change entitlement.

## Projection rules

- Order events upsert order/payment/refund summaries and trigger fulfillment only once.
- Subscription events update a provider snapshot, then recompute entitlement from policy; do not directly toggle a boolean.
- Payment events update invoice/payment history and grace/recovery inputs; do not erase a newer subscription snapshot.
- License events update key status/limits/expiry with protected identifiers.
- Customer events update allowed profile fields without using email as the identity key.

Validate `data.type`, resource ID, store/customer/subscription relationships, mode, and provider timestamps. Custom checkout data can correlate to a local account only after that account and mapping are reloaded and validated.

Keep projection and downstream effects separate. Commit the provider snapshot and entitlement atomically where possible, then emit email/provisioning/revocation work through a durable outbox.

## Entitlement examples

- cancellation may retain access until `ends_at`;
- payment failure may enter a bounded application grace period;
- recovery exits grace without duplicate provisioning;
- expiration revokes only if the event/snapshot is not older than current state;
- plan change recomputes feature/limit mapping from an allowlisted variant;
- test-mode events can never affect live entitlements.
