# Integration Architecture

## Boundaries

Prefer this flow:

```text
browser/client -> application use case -> billing provider adapter -> Lemon Squeezy
                                      -> local DB/outbox/audit
Lemon Squeezy -> verified webhook inbox -> idempotent projector -> entitlement/read model
scheduled reconciliation -------------------------------^
```

The browser receives hosted checkout/customer-portal URLs but never the provider API key. Application routes authorize the actor and tenant before resolving provider IDs.

## Suggested local model

- `billing_account`: local user/tenant, provider customer ID, store ID, mode
- `billing_catalog_mapping`: local plan key, store/product/variant/price IDs, mode, entitlement version
- `billing_subscription`: provider ID, customer/account, variant, quantity, status, pause/cancel/trial/renew/end timestamps, provider updated time, mode
- `billing_entitlement`: account, feature/limit, source resource, effective interval, policy version, decision reason
- `billing_order` / `billing_invoice`: provider IDs, currency, integer totals/refunds, status, mode, timestamps
- `billing_license`: protected or masked key reference, status, limits, expiry, related account/subscription
- `webhook_inbox`: delivery fingerprint, event/resource, mode/store, raw payload or protected reference, received/processed state, attempts/error
- `billing_outbox`: downstream side effects tied to a committed state change
- `billing_audit`: actor/system, resource, mode/store, before/after summary, reason, outcome, correlation ID

Names should follow repository conventions. Add only entities required by the product.

## Source of truth

- Lemon Squeezy: payment, order, invoice, subscription, discount, and license provider state.
- Application: identity, tenant ownership, permissions, product feature policy, local usage ledger, and final access decision.
- Local provider snapshots: fast reads and audit/recovery, not permission to overwrite newer provider truth.

Never join identities solely by mutable email. Persist provider customer/resource IDs and validate store/mode.

## Provider adapter

Expose application-oriented methods such as create checkout, fetch subscription, cancel/resume/change plan, submit usage, and fetch/refund order. Return typed domain-safe values and structured provider errors. Keep JSON:API/SDK quirks inside the adapter.

Pass request context/cancellation, set bounded timeouts, redact secrets/PII, and make the base URL injectable for tests. Do not automatically retry ambiguous mutations.

## Security and privacy

- Least-privilege API keys with owner, expiry, rotation, and health monitoring.
- Secret manager/encrypted configuration; never return secret values after saving.
- Server-side tenant/store ownership and role checks for every read/export/mutation.
- Minimize customer PII and full webhook retention; define encryption, redaction, access, and deletion policy.
- Treat signed receipt, invoice, checkout, payment-update, and portal URLs as sensitive expiring capabilities.
- Mask license keys and activation instance IDs.

## Extensibility

Keep unknown response fields tolerated and unknown event types safely ignored/recorded. Lemon Squeezy treats additive resources, optional parameters, fields, and webhook event types as backward-compatible.
