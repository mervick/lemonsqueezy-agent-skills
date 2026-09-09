# License Keys, Instances, and License API

Official sections: [License Keys](https://docs.lemonsqueezy.com/api/license-keys), [License Key Instances](https://docs.lemonsqueezy.com/api/license-key-instances), and [License API](https://docs.lemonsqueezy.com/api/license-api).

## JSON:API administration

- retrieve/update/list license keys
- retrieve/list license-key instances

Use these server-side administration resources to inspect status, activation limits/usage, expiry, related order/product/subscription, and to change only currently documented key attributes.

## License API

- `POST /v1/licenses/activate`
- `POST /v1/licenses/validate`
- `POST /v1/licenses/deactivate`

This API uses `Accept: application/json` and form-encoded POST bodies, not JSON:API. Its documented limit is 60 requests per minute.

## Security and entitlement

- Never log or expose full license keys. Prefer masked suffixes in admin UI and protected storage only when necessary.
- Treat activation instance IDs as secrets/capabilities and authorize device deactivation locally.
- Validate key status, expiry, activation limit, instance, product/variant, and related subscription according to application policy.
- Decide offline/grace behavior explicitly; a transient provider failure should not silently grant indefinite access.
- Do not send a privileged store API key from desktop/browser clients. Follow current License API authentication requirements for the exact endpoint.
- Update/deactivate/disable operations change access and require server authorization, confirmation where appropriate, and audit.
