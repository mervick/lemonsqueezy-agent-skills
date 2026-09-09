# Community Go SDK Coverage

The checked source exposes services for:

- users, stores, customers
- products, variants, prices, files
- orders and order items
- subscriptions, subscription invoices, subscription items
- discounts and discount redemptions
- license keys, license-key instances, License API operations
- checkouts
- webhook registrations and a signature helper

## Notable checked gaps/constraints

- Many `List(ctx)` methods have no filter/page arguments. `SubscriptionItems.List` and `SubscriptionInvoices.List` accept query/filter maps, but behavior is inconsistent across services.
- No checked order-refund method.
- No checked subscription-invoice refund method.
- No checked usage-record service.
- No checked customer create/update/archive methods.
- No checked license-key update method.
- No affiliate service.
- Exact API fields may lag current official docs.

Use direct HTTP for a missing capability while retaining the same application adapter and error model. Do not fork the SDK or write a second parallel billing service merely for one method.

## Service patterns

Typical methods return `(typedPayload, *Response, error)`. Preserve the response for status/body/header-aware error mapping, but redact before logging. Examples in checked source include:

- `client.Users.Me(ctx)`
- `client.Stores.Get/List`
- `client.Subscriptions.Get/List/Update/Cancel`
- `client.SubscriptionItems.Get/List/CurrentUsage/Update`
- `client.Discounts.Create/Get/List/Delete`
- `client.Checkouts.Create/Get/List`
- `client.Webhooks.Create/Get/List/Update/Delete`
- `client.Licenses.Activate/Validate/Deactivate`

Read the pinned function signature and parameter structs before coding; names and integer/string ID requirements vary.
