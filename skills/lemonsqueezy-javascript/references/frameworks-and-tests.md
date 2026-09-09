# Framework Boundaries, Webhooks, and Tests

## Server-only boundaries

- Next.js: keep the provider module in server-only code and import `server-only` where appropriate; expose authorized route handlers/server actions.
- Remix/React Router/Astro/SvelteKit/Nuxt: use server modules/endpoints and private runtime configuration.
- Cloudflare/edge runtimes: confirm Node crypto/package compatibility; use Web Crypto when Node APIs are unavailable.
- Never initialize the SDK in a shared module reachable from a client component or browser build.

Inspect production bundle/env output to verify the key is absent.

## Webhook verification (Node runtime)

```ts
import { createHmac, timingSafeEqual } from "node:crypto";

export function validSignature(raw: Buffer, receivedHex: string, secret: string): boolean {
  const expected = createHmac("sha256", secret).update(raw).digest();
  let received: Buffer;
  try { received = Buffer.from(receivedHex, "hex"); } catch { return false; }
  return received.length === expected.length && timingSafeEqual(received, expected);
}
```

Get exact raw bytes before framework JSON parsing. Persist a durable inbox/dedupe record before returning 200; process idempotently.

## Testing

- Unit-test mapping, money, entitlement, redaction, and signature bytes.
- Test the adapter with dependency-injected fetch, MSW Node, Nock, or a local server according to repository conventions.
- Assert exact method/path/query/JSON:API envelope and explicit SDK error handling.
- Cover pagination, 429, timeout, malformed response, and ambiguous mutation behavior.
- Cover duplicate/concurrent/out-of-order webhooks and cross-store/mode authorization.
- Run typecheck, tests, lint, and production build relevant to the repository. A unit test does not prove a secret stayed out of the client bundle.
