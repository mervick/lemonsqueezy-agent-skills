# Products, Variants, Prices, and Files APIs

Official sections: [Products](https://docs.lemonsqueezy.com/api/products), [Variants](https://docs.lemonsqueezy.com/api/variants), [Prices](https://docs.lemonsqueezy.com/api/prices), and [Files](https://docs.lemonsqueezy.com/api/files).

## Operations

Each resource supports retrieve and list operations in the current public API. Confirm current filters and includes on the exact endpoint page.

## Model

- Product: sellable product-level identity and status.
- Variant: a purchasable option/plan beneath a product, including status and licensing settings.
- Price: price model, amount/range, billing interval, usage aggregation, and tax behavior where applicable.
- File: fulfillment/download metadata related to a variant.

## Application use

- Keep stable local plan keys mapped to test/live store, product, variant, and optional price IDs. Business logic should not scatter raw IDs.
- Validate that mapped variants/prices belong to the expected product/store and are in a usable status.
- Do not assume the first variant or current display name is stable.
- Model free, fixed, pay-what-you-want, subscription, and usage-based price shapes explicitly. Do not force all prices into one scalar.
- Treat file download URLs as sensitive, time-bound fulfillment data. Do not proxy or persist them casually.
- Public API catalog operations are primarily read-only. Product/variant/price editing often remains a Lemon Squeezy dashboard concern.

For sync, upsert by provider ID and mode, tolerate additive fields, retain provider `updated_at`, and never let an older snapshot overwrite a newer one.
