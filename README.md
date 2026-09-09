# Lemon Squeezy Agent Skills

A modular suite of Agent Skills for building and operating Lemon Squeezy integrations across applications, websites, services, and digital products. It provides implementation-focused, safety-conscious guidance based on current Lemon Squeezy documentation and SDK source.

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

### Install from GitHub

Use the Agent Skills CLI to install from the repository:

```bash
npx skills add mervick/lemonsqueezy-agent-skills
```

The interactive installer lets you select the Lemon Squeezy skills you need, the target agent, and project-level or global installation. For Codex, select `codex`; project-level skills are installed under `.codex/skills/`, while global skills are installed under `~/.codex/skills/`.

To inspect the available skills before installing:

```bash
npx skills add mervick/lemonsqueezy-agent-skills --list
```

To install a specific skill globally for Codex without prompts:

```bash
npx skills add mervick/lemonsqueezy-agent-skills \
  --skill lemonsqueezy-integration \
  --agent codex \
  --global \
  --yes
```

Repeat `--skill <name>` to install several skills in one command. Run `npx skills check` to check for updates and `npx skills update` to update installed skills.

### Install manually

Clone the repository and copy or symlink the desired directories from `skills/` into your agent's skills directory. For Codex, the default global directory is `~/.codex/skills/`:

```bash
git clone https://github.com/mervick/lemonsqueezy-agent-skills.git
mkdir -p ~/.codex/skills
cp -R lemonsqueezy-agent-skills/skills/lemonsqueezy-integration ~/.codex/skills/
```

Replace `lemonsqueezy-integration` with another skill name, or copy all directories under `skills/` to install the complete suite. Keep each complete skill directory so its `agents/` metadata and `references/` remain available.

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
