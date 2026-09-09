# Discounts and Discount Redemptions APIs

Official sections: [Discounts](https://docs.lemonsqueezy.com/api/discounts) and [Discount Redemptions](https://docs.lemonsqueezy.com/api/discount-redemptions).

## Operations

- create, retrieve, delete, and list discounts
- retrieve and list discount redemptions

Do not fabricate a general discount update operation when current public docs expose create/read/delete only.

## Creation rules

Verify exact fields for code/name, amount type, integer amount, duration/duration months, start/expiry, redemption limit, and product/variant restrictions. Relate the new discount to the authorized store using the documented JSON:API relationship.

- Normalize and validate code according to product policy.
- Validate percentage/fixed bounds and show fixed values in store currency.
- Ensure start precedes expiry and duration combinations are valid.
- Flag 100%, unrestricted, unlimited, or long-lived discounts for extra review.
- Require explicit confirmation before a live create/delete and audit the actor/reason/effect.

## Analytics

Use discount-redemption relationships and orders rather than text matching. Report redemptions, unique customers, discount value, gross/order revenue, refunds, and net-after-refunds with currency/time scope. `ROI` requires campaign cost and an explicit attribution model; redemption revenue alone is not causal ROI.
