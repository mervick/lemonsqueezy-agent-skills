# Users and Stores API

Official sections: [Users](https://docs.lemonsqueezy.com/api/users) and [Stores](https://docs.lemonsqueezy.com/api/stores).

## Operations

- `GET /v1/users/me`: verify which Lemon Squeezy user an API key authenticates.
- `GET /v1/stores/{id}`: retrieve one accessible store.
- `GET /v1/stores`: list stores available to the authenticated user.

## Application use

- Use `users/me` plus a store read for connection testing, but expose only a sanitized identity/status to admins.
- Persist an explicit allowed store ID per integration/mode. Do not default to the first returned store.
- Treat store currency and domain/slug as display/configuration metadata, not permission proof.
- When one installation can access multiple stores, require an authorized mapping from local tenant/business to provider store.
- Connection-health tests should distinguish expired/invalid key, insufficient permissions, missing store, and wrong mode.

These are read-only API resources. Do not promise store or user settings edits through this public API unless current documentation adds them.
