# Protocol and Cross-Cutting Contract

## Main API

- Origin: `https://api.lemonsqueezy.com`
- Versioned paths: `/v1/...`
- Authentication: `Authorization: Bearer <api-key>`
- `Accept: application/vnd.api+json`
- `Content-Type: application/vnd.api+json`
- Create/update/action bodies follow the endpoint's JSON:API `data` envelope with exact `type`, optional string `id`, `attributes`, and required relationships.

API keys are created separately in test and live modes and are valid for a limited period. Make rotation observable and do not infer mode only from an environment variable name.

## Pagination and querying

- List endpoints use `page[number]` and `page[size]`; documented size range is 1–100.
- For complete results follow top-level `links.next` until absent. Validate the origin before following an absolute URL.
- `meta.page` reports current/last page and totals; do not use totals as if records were fetched.
- Filters use `filter[field]=value`; includes use `include=relationship`. Exact support is endpoint-specific.
- Do not assume date filters or sorting exist. If unavailable, use a webhook-fed local reporting model or fully paginate then filter.

## Errors and limits

Parse JSON:API error documents and retain sanitized status/code/title/detail/source plus an internal correlation ID. Distinguish authentication, authorization, not-found/mismatched resource, validation, rate limiting, provider failure, timeout, and malformed response.

The checked main API docs state a 300 requests/minute limit and expose `X-Ratelimit-Limit` and `X-Ratelimit-Remaining`; 429 indicates exhaustion. Use bounded exponential backoff with jitter for safe reads and honor `Retry-After` if supplied. Reconcile state before retrying an ambiguous mutation.

## Money, time, and mode

- Money fields are generally integer minor units; confirm each field and keep currency alongside it.
- Use provider ISO 8601 timestamps, store UTC, and apply an explicit reporting time zone.
- Persist/check `test_mode` where returned. Prevent test data from granting production entitlements or polluting live reports.

## License API exception

`/v1/licenses/activate`, `/validate`, and `/deactivate` are a separate API contract:

- `Accept: application/json`
- POST body: `application/x-www-form-urlencoded`
- documented limit: 60 calls/minute

Do not pass these calls through a JSON:API serializer.

## Verification checklist

- exact method/path and `type`
- supported attributes, relationships, filters, includes, and status values
- store/mode/tenant ownership
- pagination completeness
- minor-unit currency handling
- timeout/error/rate-limit behavior
- safe retry or explicit ambiguity path
- redaction and audit behavior
