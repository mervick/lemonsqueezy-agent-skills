# SDK Resource Map

The checked v4 source exports these groups. Search installed declarations for exact signatures.

| Area | Functions in checked source |
| --- | --- |
| Setup/user | `lemonSqueezySetup`, `getAuthenticatedUser` |
| Stores | `getStore`, `listStores` |
| Customers | `createCustomer`, `getCustomer`, `updateCustomer`, `archiveCustomer`, `listCustomers` |
| Catalog | `get/listProduct`, `get/listVariant`, `get/listPrice`, `get/listFile` |
| Orders/items | `getOrder`, `listOrders`, `generateOrderInvoice`, `issueOrderRefund`, `getOrderItem`, `listOrderItems` |
| Subscriptions | `getSubscription`, `listSubscriptions`, `updateSubscription`, `cancelSubscription` |
| Subscription invoices | get/list, generate invoice, issue invoice refund |
| Subscription items | get/list, current usage, update |
| Usage | `createUsageRecord`, `getUsageRecord`, `listUsageRecords` |
| Discounts | `createDiscount`, `getDiscount`, `listDiscounts`, `deleteDiscount` |
| Discount redemptions | get/list |
| License keys/instances | get/list keys, update key, get/list instances |
| License API | `activateLicense`, `validateLicense`, `deactivateLicense` |
| Checkouts | `createCheckout`, `getCheckout`, `listCheckouts` |
| Webhook registrations | create/get/update/delete/list |

The API may expose newer areas such as affiliates before a locked SDK version does. Use a project-local direct HTTP method behind the same adapter for missing coverage rather than replacing the entire SDK.

## Important signatures

- `createCheckout(storeId, variantId, checkout?)` builds store/variant relationships; resolve IDs from server-side allowlisted mappings.
- `updateSubscription(id, update)` supports fields such as variant, cancelled, billing anchor, invoice immediately, disable prorations, pause, and trial end in checked v4.
- `cancelSubscription(id)` issues the provider cancellation request; cancellation may retain access until `ends_at`.
- `issueOrderRefund(id, amount)` requires an explicit integer minor-unit amount in checked v4 even though direct HTTP permits omitted amount for full refund.
- License functions use the separate License API contract internally; inspect whether a store API key is required for the exact version/endpoint.

## List example

```ts
const result = await listSubscriptions({
  filter: { storeId, userEmail: normalizedEmail, status: "active" },
  include: ["subscription-items"],
  page: { number: 1, size: 100 },
});
if (result.error) throw mapProviderError(result.error, result.statusCode);
```

Only use filters/includes permitted by the installed type and current endpoint. A first page is not an all-time report.
