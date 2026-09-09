# Orders and Order Items APIs

Official sections: [Orders](https://docs.lemonsqueezy.com/api/orders) and [Order Items](https://docs.lemonsqueezy.com/api/order-items).

## Operations

- retrieve/list orders
- generate an order invoice
- issue a partial or full order refund
- retrieve/list order items

The documented refund action is `POST /v1/orders/{id}/refund`. Omitting `amount` requests a full refund; an explicit integer minor-unit amount requests a partial refund. SDK helpers may require an amount even when direct HTTP does not.

## Correct modeling

- Keep order number/identifier distinct from provider order resource ID.
- Store currency with subtotal, discount, tax, total, and refunded amount.
- Use `refunded_amount` for partial refunds; `refunded` alone is insufficient.
- Use order items for product/variant breakdown rather than relying only on `first_order_item`.
- Treat receipt/invoice URLs as sensitive opaque links that may expire.
- Include `test_mode`, created/updated timestamps, status, customer/store IDs, and related subscription/license IDs needed by the app.
- Invoice generation uses customer address fields in query parameters and the current docs warn that fields previously treated as optional are becoming required. Validate all documented required fields in application code rather than depending on older SDK optionality.

## Refund safety

Before a live refund, server-authorize actor/account/store, fetch current order, compute remaining refundable amount, show exact currency/amount/customer/order, require explicit confirmation and reason, create an audit intent, execute once, then verify by fresh read or webhook. A timeout is `unknown`; reconcile before retrying.

The current public Orders API navigation does not document `POST /orders/{id}/resend-receipt`. Do not implement that action unless an official current endpoint confirms it. Offer the receipt URL or provider dashboard workflow instead.
