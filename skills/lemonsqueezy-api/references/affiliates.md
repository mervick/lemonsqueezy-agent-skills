# Affiliates API

Official section: [Affiliates](https://docs.lemonsqueezy.com/api/affiliates/the-affiliate-object).

## Operations

The current API exposes retrieve and list operations. The checked list endpoint documents `store_id` and `user_email` filters; confirm current includes, statuses, and fields on the exact endpoint page.

## Application use

- Scope affiliates to the expected store/mode and treat provider IDs as external references.
- Keep affiliate status, commission metadata, URLs, and payout-related display data separate from application user identity.
- Do not expose customer/order PII merely because an operator can view an affiliate.
- For analytics, state whether revenue is attributed by a provider relationship, discount code, UTM/custom data, or another method.
- Do not calculate ROI or payable commissions from an undocumented approximation. Lemon Squeezy as Merchant of Record may remain authoritative for payouts and tax handling.

This is read-only integration unless current official docs explicitly add mutations.
