---
name: lemonsqueezy-javascript
description: Implement, debug, migrate, or review Lemon Squeezy integrations in server-side JavaScript or TypeScript using the official @lemonsqueezy/lemonsqueezy.js SDK, including every SDK resource group, checkout, subscriptions, refunds, usage, licenses, webhooks, errors, and framework boundaries.
---

# Lemon Squeezy JavaScript SDK

Use the official [`@lemonsqueezy/lemonsqueezy.js`](https://github.com/lmsqueezy/lemonsqueezy.js) when it fits the repository. The checked source was v4.0.0, ESM/CJS-capable, tree-shakeable, and declared Node.js 20+; inspect the project's lockfile and installed declarations for its actual version.

## Workflow

1. Inspect package manager, runtime, server/client module boundaries, existing API wrapper, and locked SDK version.
2. Search installed types/source for the exact function and params. Do not copy current-v4 names into an older lockfile blindly.
3. Initialize once in a server-only module, translate SDK results into application types/errors, and keep provider details behind a service boundary.
4. Use `$lemonsqueezy-api` for exact resource semantics and `$lemonsqueezy-webhooks` for delivery processing.

Read [references/setup-and-errors.md](references/setup-and-errors.md) for initialization and result handling, [references/sdk-map.md](references/sdk-map.md) for the SDK by resource, and [references/frameworks-and-tests.md](references/frameworks-and-tests.md) for webhook/framework/test patterns.

## Invariants

- Never import the secret-bearing SDK module into browser/client code. Do not use `NEXT_PUBLIC_`, `VITE_`, or other public environment variables for the API key.
- SDK functions commonly return `{ data, error, statusCode }`-style results; inspect types and handle `error` explicitly instead of relying only on thrown exceptions.
- List params use camelCase filters/page/includes that the SDK converts to API query keys. Follow pagination for complete results.
- The SDK does not make a browser request authorized merely because it is type-safe. Server authorization/store/mode checks remain mandatory.
- The SDK does not replace incoming webhook signature verification.
- Do not execute live mutations during implementation/testing without exact explicit user confirmation.

These notes were checked against the repository at commit `b1f66e905ee0614be87c3711d6529f2582e5729f` on 2026-09-09. Verify current upstream and installed code when precision matters.
