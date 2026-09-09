# Lemon Squeezy Skills for Codex

A modular Codex skill suite for building and operating Lemon Squeezy integrations across applications, websites, services, and digital products. It provides implementation-focused, safety-conscious guidance based on current Lemon Squeezy documentation and SDK source.

## Skills

| Skill | Use it for |
| --- | --- |
| `lemonsqueezy-integration` | End-to-end integration architecture, billing lifecycle, entitlements, migration, and go-live |
| `lemonsqueezy-api` | Direct REST API work, split into references for every API resource group |
| `lemonsqueezy-webhooks` | Signature verification, durable inboxes, idempotent projection, replay, and reconciliation |
| `lemonsqueezy-javascript` | Official `@lemonsqueezy/lemonsqueezy.js` SDK and JS/TS framework boundaries |
| `lemonsqueezy-go` | Community Go SDK coverage, gaps, direct-HTTP fallbacks, and Go tests |
| `lemonsqueezy-python` | Typed Python/httpx integration and accurate treatment of `lemonsqueepy` |
| `lemonsqueezy-admin` | Internal billing admin panels, settings, support actions, reporting, and audit |

Each skill uses progressive disclosure: `SKILL.md` holds routing and invariants, while `references/` contains resource- or workflow-specific detail.

## Installation

Copy or symlink the desired skill directories into the Codex skills directory (normally `$CODEX_HOME/skills`, or `~/.codex/skills` when `CODEX_HOME` is unset). Keep the complete directory for each skill so its `agents/` metadata and `references/` remain available.

The skills intentionally do not contain credentials or scripts that call a live store.

## Invocation examples

```text
Use $lemonsqueezy-integration to add subscription billing and entitlements to this application.
Use $lemonsqueezy-api to implement fully paginated order synchronization.
Use $lemonsqueezy-webhooks to make this webhook endpoint idempotent.
Use $lemonsqueezy-javascript to add a server-only checkout flow.
Use $lemonsqueezy-go to implement the missing refund operation safely.
Use $lemonsqueezy-python to add a FastAPI Lemon Squeezy client.
Use $lemonsqueezy-admin to design the billing operations settings pages.
```

## Sources and freshness

The suite was checked on 2026-09-09 against:

- [Lemon Squeezy API reference](https://docs.lemonsqueezy.com/api)
- [Lemon Squeezy developer guide](https://docs.lemonsqueezy.com/guides/developer-guide)
- [Official JavaScript SDK](https://github.com/lmsqueezy/lemonsqueezy.js)
- [Community Go SDK](https://github.com/NdoleStudio/lemonsqueezy-go)
- [Community Python project](https://github.com/mthli/lemonsqueepy)

Important current distinctions are preserved in the skills: the main API documents 300 requests per minute, the License API separately documents 60, receipt resend is a dashboard workflow rather than a documented public Orders API action, the Go SDK has meaningful coverage gaps, and `lemonsqueepy` is a standalone application rather than a conventional Python SDK.

Always re-check current official endpoint documentation and the exact locked SDK version before relying on drift-prone fields or mutations.
