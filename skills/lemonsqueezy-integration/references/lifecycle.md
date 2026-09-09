# Billing Lifecycle

## Provisioning and checkout

1. Resolve authenticated local account/tenant and stable plan key.
2. Resolve allowlisted test/live store and variant/price mapping on the server.
3. Create a hosted checkout with validated quantity/custom-price/discount/redirect options and documented custom data for local correlation.
4. Return only the safe hosted URL; do not grant access from a browser success redirect.
5. Grant/update access after a verified provider webhook or reconciled provider read.

## Subscription state and access

Do not reduce billing to `is_active`. Preserve provider status plus trial, pause, cancelled, renew/end timestamps and application policy. Define access for:

- active and trial subscriptions;
- cancellation with access through `ends_at`;
- expired subscriptions;
- pause modes (`void` and `free` have different business meaning);
- payment failure/recovery and optional grace;
- plan/quantity changes and proration;
- refunds or charge state that may affect entitlement.

Make entitlement evaluation deterministic and versioned. A result should explain source, policy, effective/expiry time, and any grace.

## Customer self-service

Prefer provider customer portal URLs for payment method and supported subscription self-service. Provider URLs are signed/expiring capabilities: fetch just in time, authorize the local account first, and do not log/cache broadly. PayPal subscription plan changes may require `customer_portal_update_subscription` rather than direct API update.

## Usage billing

Keep a durable local usage ledger and map a meter to the correct subscription item. Define units, aggregation action, billing-period boundary, late data, correction, and backfill. Submit through an outbox. After a timeout, reconcile current usage/records before retrying.

## Licensing

Separate JSON:API license-key administration from form-encoded License API activation/validation/deactivation. Define activation ownership, limits, expiry, offline/grace behavior, and relationship to subscription state. Never log full keys or expose privileged API credentials to distributed clients.

## Refunds and cancellations

Use a fresh provider read, exact remaining amount/state, server authorization, reason, explicit confirmation for live actions, mutation audit, single execution, and fresh-read/webhook verification. Treat timeouts as unknown, not failed.

## Downtime policy

Choose deliberately whether the product fails closed, uses a short cached entitlement, or enters grace when provider reads/webhooks are unavailable. Keep the policy bounded and observable; provider outage must not create indefinite access or immediate mass revocation.
