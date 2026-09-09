# Checkouts API

Official section: [Checkouts](https://docs.lemonsqueezy.com/api/checkouts).

## Operations

- `POST /v1/checkouts`: create a checkout related to a store and variant.
- retrieve/list checkouts.

## Secure application pattern

1. Browser sends a stable local plan key and allowed quantity/options.
2. Server authorizes the local account/tenant and resolves an allowlisted test/live store/variant mapping.
3. Server validates custom price, quantity, discount, and redirect policy.
4. Server creates the checkout and returns only the provider checkout URL and safe expiry/display data.
5. Verified webhooks, not the browser redirect alone, activate durable entitlements.

Use documented checkout custom data to carry local user/tenant/correlation identifiers. After signature verification, still validate those identifiers and provider store/mode/variant; custom data is context, not authorization.

Do not accept raw store/variant IDs, arbitrary redirect URLs, arbitrary JSON attributes, or price values from the client. Do not handle card data. Treat created checkout URLs as opaque and potentially expiring.

Model preview/test mode, product options, checkout options, checkout data, custom price, quantity/variant quantities, and expiry only when supported by the current endpoint/schema or locked SDK types.
