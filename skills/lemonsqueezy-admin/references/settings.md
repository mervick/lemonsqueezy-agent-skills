# Lemon Squeezy Settings

## Connection

- unmistakable test/live mode and separate configuration
- API key secret reference, masked fingerprint, expiry/rotation owner/date
- test connection using authenticated-user and selected-store reads
- explicit allowed store ID/name/slug/currency/mode; never default to first store
- last verification, status, sanitized error, rate-limit state

Saving/replacing/clearing a key is server-only, validated, permission-gated, and audited. Never echo the key.

## Catalog and entitlements

- local plan key to store/product/variant/price mapping per mode
- feature/limit/quantity/meter/license mapping and policy version
- provider snapshot and last verified time
- duplicate, missing, cross-store, wrong-mode, inactive-variant validation

Fail closed on unknown mappings.

## Checkout

- allowlisted variants, quantities, custom-price/discount policy
- approved redirect/success URLs
- documented custom-data keys for local identity/correlation
- checkout presentation options supported by current API
- signed/expiring URL handling

Never accept arbitrary provider IDs, redirects, attributes, or price from the browser.

## Webhooks and synchronization

- public HTTPS endpoint, selected handled events, registration/store/mode
- secret reference/fingerprint and coordinated rotation state
- last delivery/success, queue age, failures, unknown/signature failures
- reconciliation cadence/scope/page/concurrency, backfill/dry-run, retention/redaction
- last/next run and scanned/changed/failed/drift counts

A partial job cannot be labeled successful without a partial warning.

## Billing policy

- cancellation access through `ends_at`
- payment-failure grace and recovery
- trial and pause behavior
- customer portal versus admin-managed changes
- proration defaults only if product policy explicitly defines them
- refund roles/reason policy
- license activation/offline/expiry policy

Version/audit policy changes and test entitlement behavior separately from labels.

## Security, retention, and alerts

- roles for PII view/export, refunds, subscription/discount/license operations, replay, settings
- step-up authentication where supported, CSRF/session protection, secret-access audit
- payload/audit/export retention and PII redaction
- alerts for key expiry/auth failure, rate limit, webhook silence/failure, queue age/dead letters, drift/stale sync, payment failures, unusual refunds/discounts

Provide permission-gated Test connection, Send test/simulate, Test alert, Reconcile, and Replay actions with audit.
