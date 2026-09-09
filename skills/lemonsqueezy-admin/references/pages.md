# Admin Information Architecture

Ship only areas the product supports; disabled actions must explain why.

## Overview

- connection/store/mode health, rate-limit headroom, last webhook/sync
- revenue/orders/subscriptions summary with date/currency/time-zone/completeness
- actionable failures: payments, expiring trials/licenses, webhook failures, drift
- recent orders and audit activity

Link cards to the exact filtered records. Do not replace failed metrics with zero.

## Catalog and plans

- stable local plan key and test/live provider store/product/variant/price IDs
- provider name/status/currency/interval/usage/license snapshot
- checkout enabled state and entitlement mapping
- last verified/synced and mapping validation errors

Catalog provider data is mainly read-only through the public API; local mapping edits are audited application configuration.

## Customers

- search by local account/tenant, provider customer ID, order number, or normalized email
- identity link and duplicate/mismatch warning
- orders, subscriptions, invoices, licenses, currency-grouped totals, timeline
- allowed support actions and source freshness

Email is context, not authorization or durable identity.

## Orders and invoices

- order number/resource ID, customer, time, status, line items, currency, total, refunded amount, mode
- tax/discount/refund breakdown, receipt/invoice link, related subscription/license
- refund/invoice actions only after backend eligibility

Represent partial refunds explicitly.

## Subscriptions and usage

- provider status plus application entitlement decision
- plan/quantity, trial/renew/end, pause/cancel, payment failure/recovery, mode
- invoices, usage/current period, history, customer portal, allowed actions

Explain grace or access-through-end rather than flattening to active/inactive.

## Discounts, licenses, webhooks

- discounts: code/type/amount/duration/window/limits/restrictions/redemptions and attribution method
- licenses: masked key, status, product/order/account, activation count/limit, expiry, instances
- webhooks: registrations, event set, inbox outcomes/attempts/errors, queue age, replay/reconcile

## Interaction contract

- Server-side pagination for large sets; bounded, explicit selection across pages.
- Filters in URL only when privacy-safe; never put secrets/full PII in shareable URLs.
- Row navigation opens detail; mutations use explicit labeled controls.
- Refresh provider state or show pending verification after mutation; do not optimistically claim final state.
- Provide distinct loading, empty, stale, partial, rate-limited, unavailable, sync-failed, and forbidden states.
