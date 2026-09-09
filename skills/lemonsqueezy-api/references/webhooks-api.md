# Webhooks API

Official section: [Webhooks](https://docs.lemonsqueezy.com/api/webhooks).

## Registration operations

- create a webhook with URL, selected event names, signing secret, store relationship, and test-mode setting where documented
- retrieve, update, delete, and list registrations

The secret is write-only and is not returned by the API. Keep the application's secret reference and provider registration lifecycle coordinated.

## Configuration rules

- Use a public HTTPS URL owned by the application.
- Select only events the application handles.
- Separate registrations/secrets by mode and store when required.
- Validate redirect/domain configuration and prevent arbitrary callback registration through user input.
- Audit create/update/delete and require explicit approval for live registration changes.
- Plan secret rotation. If dual-secret overlap is unavailable, coordinate deployment and provider update to avoid rejected deliveries.

The Webhooks API manages registrations; incoming delivery verification, durable inbox, idempotency, ordering, and replay belong in the `lemonsqueezy-webhooks` skill.
