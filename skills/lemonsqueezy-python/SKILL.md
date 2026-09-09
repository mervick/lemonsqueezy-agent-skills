---
name: lemonsqueezy-python
description: Implement, debug, migrate, or review Lemon Squeezy integrations in Python using a typed httpx client and secure webhook handling, and evaluate mthli/lemonsqueepy accurately. Use for FastAPI, Django, Flask, Quart, workers, direct REST APIs, subscriptions, usage, licensing, and tests.
---

# Lemon Squeezy for Python

There is no official Python SDK in the checked Lemon Squeezy list. [`mthli/lemonsqueepy`](https://github.com/mthli/lemonsqueepy) is listed as a community Python project, but the checked code is a standalone 2023 Quart account/payment service tied to MongoDB, Redis, and Google OAuth—not a normal pip-installable API client.

Do not add or vendor `lemonsqueepy` by default. Use it only as reviewed reference material. For normal projects, build a small typed async or sync adapter with the repository's existing HTTP stack, usually `httpx`.

## Workflow

1. Inspect framework lifecycle, async/sync model, existing HTTP client, schemas, configuration, tasks/queues, and tests.
2. Use [references/http-client.md](references/http-client.md) for main/License API clients and [references/frameworks-and-webhooks.md](references/frameworks-and-webhooks.md) for framework/security/testing.
3. Use `$lemonsqueezy-api` for resource-specific endpoint behavior and `$lemonsqueezy-webhooks` for reliable event processing.

## Invariants

- Keep one lifecycle-managed client with explicit timeouts/limits; do not instantiate a client per request.
- Model needed fields and tolerate additive response fields.
- Validate next-page origins before following absolute pagination URLs.
- Keep secrets server-side and never log full payloads, keys, signed URLs, or license values.
- Use exact raw webhook bytes and `hmac.compare_digest`.
- Do not automatically retry ambiguous mutations.
- Do not execute live mutations during implementation/testing without exact explicit confirmation.

Checked `lemonsqueepy` commit: `3bc9a4b92abe52582c5b06ec171c7e962e8d09d8` (2023-08-23). Treat its endpoint/event assumptions as historical and verify current official docs.
