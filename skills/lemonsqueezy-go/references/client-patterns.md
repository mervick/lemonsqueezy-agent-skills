# Go Client, Fallbacks, Webhooks, and Tests

## Client

```go
client := lemonsqueezy.New(
    lemonsqueezy.WithAPIKey(cfg.APIKey),
    lemonsqueezy.WithSigningSecret(cfg.WebhookSecret),
    lemonsqueezy.WithHTTPClient(httpClient),
)

subscription, response, err := client.Subscriptions.Get(ctx, subscriptionID)
if err != nil {
    return mapProviderError(response, err)
}
```

Construct the client with `New`; do not instantiate `Client{}`. Keep one reusable client per mode/configuration. Use explicit HTTP client timeouts and pass request contexts.

## Direct HTTP fallback

- Keep base URL injectable and append exact `/v1/...` paths.
- Set bearer auth plus both JSON:API media headers.
- Use typed envelope structs rather than nested maps in business logic.
- Encode filter/page/include query values with `net/url`.
- Parse JSON:API errors with status and sanitized details.
- Implement only the missing operation behind the existing adapter.

## Webhook verification

The checked SDK helper compares hex strings with `==`. Prefer constant-time bytes:

```go
func validSignature(raw []byte, signatureHex, secret string) bool {
    received, err := hex.DecodeString(signatureHex)
    if err != nil { return false }
    mac := hmac.New(sha256.New, []byte(secret))
    _, _ = mac.Write(raw)
    return hmac.Equal(mac.Sum(nil), received)
}
```

Cap/read raw body, verify, then decode the saved bytes. Never use `log.Fatal` in a request handler. Persist/dedupe before 200.

## Tests

Use `httptest.Server` with injected base URL/client to assert method, path, query, headers, JSON envelope, response mapping, context cancellation, timeout, and every-page traversal. Test HMAC bytes, duplicate/concurrent webhook delivery, and ambiguous mutations. Run `go test ./...`; use `-race` for shared webhook/job paths when feasible.
