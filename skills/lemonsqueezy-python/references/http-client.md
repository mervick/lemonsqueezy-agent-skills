# Python HTTP Client

## Async adapter

```python
import httpx


class LemonSqueezyClient:
    def __init__(self, api_key: str, client: httpx.AsyncClient) -> None:
        self._client = client
        self._headers = {
            "Authorization": f"Bearer {api_key}",
            "Accept": "application/vnd.api+json",
            "Content-Type": "application/vnd.api+json",
        }

    async def get_subscription(self, subscription_id: str) -> dict:
        response = await self._client.get(
            f"/v1/subscriptions/{subscription_id}", headers=self._headers
        )
        raise_provider_error(response)
        return response.json()
```

Create `httpx.AsyncClient(base_url="https://api.lemonsqueezy.com", timeout=..., limits=...)` in application lifespan and close it on shutdown. In sync applications use one managed `httpx.Client` or the existing client abstraction.

## JSON:API

Build typed/Pydantic request envelopes with `data.type`, string `id` where required, `attributes`, and relationships. Parse only fields needed by the application while allowing unknown additions. Convert provider errors into typed application exceptions with sanitized detail/status.

## Pagination

```python
async def iter_data(client, path, *, headers, params):
    url = path
    first = True
    while url:
        response = await client.get(url, headers=headers, params=params if first else None)
        raise_provider_error(response)
        payload = response.json()
        yield from payload["data"]
        url = payload.get("links", {}).get("next")
        validate_api_origin(url)
        first = False
```

Use complete pagination only when needed; stream/process pages instead of loading unbounded results.

## License API

License activation/validation/deactivation uses `Accept: application/json` and form data, not JSON:API:

```python
response = await client.post(
    "/v1/licenses/validate",
    headers={"Accept": "application/json"},
    data={"license_key": license_key, "instance_id": instance_id},
)
```

Check current auth/field requirements and never log key/instance values.

## Retry and ambiguity

Retry bounded safe reads for transient/rate-limit failures with jitter using existing project facilities. Do not blindly retry refund, usage, checkout, subscription, or discount mutations after a timeout; reconcile provider state.
