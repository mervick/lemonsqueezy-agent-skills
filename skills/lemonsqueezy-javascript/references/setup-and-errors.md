# Setup, Results, and Errors

## Install and initialize

Use the repository's package manager:

```bash
npm install @lemonsqueezy/lemonsqueezy.js
```

```ts
import { getAuthenticatedUser, lemonSqueezySetup } from "@lemonsqueezy/lemonsqueezy.js";

lemonSqueezySetup({
  apiKey: env.LEMONSQUEEZY_API_KEY,
  onError: (error) => logger.error({ error: sanitize(error) }, "Lemon Squeezy error"),
});

const result = await getAuthenticatedUser();
if (result.error) throw mapProviderError(result.error, result.statusCode);
```

Initialize once per process/server runtime in a server-only module. Validate a non-empty key at startup or feature initialization without printing it.

## Adapter pattern

Return application-facing values rather than SDK response envelopes from route handlers. Normalize:

- authentication/permission/not-found/validation/rate-limit/provider/timeout errors;
- provider IDs to opaque strings;
- money to integer minor units plus currency;
- provider timestamps to validated UTC values;
- `test_mode` and store ownership.

Preserve useful sanitized provider detail and an internal correlation ID. Never return the key, raw signed URL, full PII, request headers, or full webhook payload in generic errors.

## Pagination

List functions accept `page: { number, size }`, `filter`, and `include` when the resource types allow them. Inspect returned `links.next`/`meta.page`. Build a reusable iterator only if multiple use cases require complete traversal; validate any absolute next URL remains on `api.lemonsqueezy.com`.

## Retry policy

Use bounded reads with backoff/jitter for 429/transient failures. Do not blindly retry refunds, usage records, checkouts, plan changes, cancellations, or other ambiguous mutations. Re-fetch provider state first.

## Version discipline

- Prefer the lockfile and installed `.d.ts` over upstream examples.
- Use exact imports from the package root; it is tree-shakeable.
- Do not reach into undocumented internal paths.
- When upgrading, review changelog, Node requirement, changed types, response behavior, and generated browser bundles.
