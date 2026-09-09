# Python Frameworks, Webhooks, and Tests

## Framework placement

- FastAPI/Starlette: create the async client in lifespan; read `await request.body()` before JSON parsing; queue processing after durable insert.
- Django: use a service layer and background worker; read `request.body`; keep sync/async boundaries explicit.
- Flask: use an application-owned client/service and `request.get_data(cache=True)` before JSON parsing.
- Quart: use lifespan/startup hooks and raw body APIs; do not copy `lemonsqueepy`'s Mongo/Redis/OAuth architecture unless the product actually needs it.

## Signature verification

```python
import hashlib
import hmac


def valid_signature(raw: bytes, signature: str, secret: str) -> bool:
    expected = hmac.new(secret.encode(), raw, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, signature)
```

Reject missing/invalid signatures before parsing or side effects. Do not include received/computed signatures in errors. Validate event header/meta, store, mode, and local mapping; persist/dedupe before 200.

## `lemonsqueepy` evaluation

The checked repository demonstrates:

- HMAC-SHA256 with `hmac.compare_digest`;
- Quart webhook routing;
- form-encoded license activation with `httpx`;
- local order/subscription/license projections.

It also embeds application-specific Redis secret access, Mongo models, Google OAuth, event lists, retry assumptions, and logging choices. Its checked code logs full bodies/license data and was last committed in 2023. Do not copy those behaviors into production.

## Tests

Use repository conventions such as `pytest`, `pytest-asyncio`, `respx`, `httpx.MockTransport`, Django/ASGI/Flask test clients. Cover exact JSON:API requests, errors, pagination-origin checks, timeouts, raw signature bytes, duplicates/concurrency/order, wrong store/mode, redaction, and ambiguous mutation outcomes.
