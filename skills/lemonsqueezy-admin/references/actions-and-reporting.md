# Actions, Support, and Reporting

## Shared mutation sequence

1. Server authorizes actor, tenant, store, mode, and resource.
2. Server fetches fresh provider state and eligibility.
3. Review UI shows identity, current/requested state, immediate/future effects, money/currency, and mode.
4. Operator supplies required reason and confirms.
5. UI disables duplicate submit; server creates an audit/correlation intent.
6. Backend executes once. A timeout becomes unknown, not failed.
7. Audit stores actor, before/after summary, reason, outcome, correlation.
8. Fresh read/webhook verifies state; UI distinguishes accepted, verified, failed, and unknown.

## Refunds

Show order/invoice number/ID, masked customer, original total, already refunded, remaining, exact currency/amount, and items. Use separate Full and Partial choices so blank amount is never ambiguous. Convert to minor units once and revalidate server-side. Reconcile before retry after an unknown result.

## Subscription actions

For cancel/resume/pause/unpause/plan/trial/billing-anchor changes, show effective date, access impact, current/new plan/interval/price, renewal/end, proration choice, and possible immediate invoice. Never silently choose proration flags. Use provider portal links for payment method and PayPal-required changes.

## Discounts, receipts, licenses, replay

- Discounts: validate amount/duration/window/limits/restrictions; flag 100%/unlimited/broad; confirm live create/delete. Do not invent update support.
- Receipts: the current public API exposes receipt URLs and invoice generation. Do not implement a resend-receipt API action without an officially documented endpoint.
- Licenses: mask values, authorize instance/key ownership, show access impact, confirm mutations.
- Replay: only stored verified deliveries, normal idempotent handler, current-state preview, actor/reason audit.

## Metric contract

Every metric defines source, store/mode, statuses, timestamp and reporting zone, currency, tax/discount/refund treatment, freshness, and completeness.

- Fully paginate or use a reconciled reporting store; first 100 is not all time.
- Group currencies; label any FX source/rate/time.
- Use `refunded_amount` for partial refunds.
- Conversion needs a real denominator; ROI needs campaign cost and attribution model.
- Link dashboard values to the exact filtered records.

## Customer support and exports

Customer lookup uses local/provider IDs, order number, and normalized email with duplicate/mismatch warnings. Minimize PII and require product-defined identity verification before disclosing signed links or changing access.

Exports reuse server permissions/scope/masking, record actor/filter/row count/outcome, stream large jobs, and include generated time, zone, currency, source/freshness, and partial warnings.
