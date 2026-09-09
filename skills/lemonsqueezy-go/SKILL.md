---
name: lemonsqueezy-go
description: Implement, debug, migrate, or review Lemon Squeezy integrations in Go using github.com/NdoleStudio/lemonsqueezy-go or a typed direct-HTTP adapter, including SDK resource coverage, gaps, webhooks, tests, errors, pagination, and safe application boundaries.
---

# Lemon Squeezy for Go

[`github.com/NdoleStudio/lemonsqueezy-go`](https://github.com/NdoleStudio/lemonsqueezy-go) is a community SDK, not official. Inspect the pinned module and source before relying on its coverage; use direct HTTP behind the same adapter when a required endpoint or query option is missing.

## Workflow

1. Inspect `go.mod`, existing HTTP client/provider adapter, configuration, contexts, errors, and tests.
2. Compare the required operation with [references/sdk-coverage.md](references/sdk-coverage.md) and the pinned source.
3. Follow [references/client-patterns.md](references/client-patterns.md) for initialization, fallbacks, webhook verification, and testing.
4. Use `$lemonsqueezy-api` for endpoint semantics and `$lemonsqueezy-webhooks` for delivery reliability.

## Invariants

- Reuse a configured `http.Client` with timeouts and transport limits; pass request contexts.
- Keep API keys/signing secrets server-side and store/mode scoped.
- Map SDK structs/errors to application types at an adapter boundary.
- Do not assume all `List` methods accept filters or pagination.
- Do not use the checked SDK webhook `Verify` helper as the final security boundary unless its implementation uses constant-time comparison in the pinned version.
- Do not execute live mutations during code/test work without exact explicit confirmation.

Checked upstream commit: `44297d4cfa1d0fc4c155944843cc91c7f15e894a` on 2026-09-09. Re-check pinned/current code for exact behavior.
