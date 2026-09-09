# Subscriptions and Subscription Invoices APIs

Official sections: [Subscriptions](https://docs.lemonsqueezy.com/api/subscriptions) and [Subscription Invoices](https://docs.lemonsqueezy.com/api/subscription-invoices).

## Subscription operations

- `GET /v1/subscriptions/{id}` and list subscriptions.
- `PATCH /v1/subscriptions/{id}` to change documented attributes such as variant, pause, cancelled/resumed state, trial end, billing anchor, and proration controls.
- `DELETE /v1/subscriptions/{id}` to cancel an active subscription.

Plan changes may prorate, invoice immediately, change billing cycle/date, create credits, or end/start a trial. Never silently choose `invoice_immediately` or `disable_prorations`. For PayPal, direct changes may not apply; use `urls.customer_portal_update_subscription` when returned.

Cancellation is not always immediate expiration. Preserve `cancelled`, `ends_at`, `renews_at`, pause, trial, and provider status separately. Resuming may be possible by setting `cancelled: false` before `ends_at`, subject to current rules.

## Invoice operations

- retrieve/list subscription invoices
- generate an invoice document/link
- issue a partial/full subscription invoice refund

Apply the same minor-unit, remaining-refundable, authorization, confirmation, ambiguity, and audit rules as order refunds.

## Entitlements

Do not map the lifecycle to a single active boolean. Define policy for trial, active, paused modes, cancelled-until-end, failed/recovered payments, grace, and expired. Webhooks update the local projection; reconciliation repairs drift. Ignore older provider snapshots when a newer `updated_at` is already applied.
