# List Organization Webhooks

Returns all active webhook subscriptions that belong to the authenticated
organization. The response is not paginated and its order is not guaranteed.

See [Organization Webhooks](README.md) for subscription types, channels, and
metadata.

## Request

**Method:** `GET`

**URL:** `https://sdk.mediquo.com/v1/organizations/webhooks/`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | API key of an API v1 organization |
| `x-secret-key` | Yes | Secret key of the same organization |
| `Accept` | Recommended | Set to `application/json` |

### Body

This endpoint does not accept a request body.

## Responses

### `200 OK`

```json
{
  "data": [
    {
      "id": "c0766228-bde8-447e-8284-040e79e952a8",
      "type": "report_created",
      "channel": "http",
      "meta": {
        "url": "https://client.example.com/hooks/report"
      }
    }
  ]
}
```

If the organization has no active subscriptions, `data` is an empty array.

| Field | Type | Description |
|-------|------|-------------|
| `data` | `array<object>` | Active subscriptions owned by the authenticated organization |
| `data[].id` | `string` (UUID v4) | Subscription identifier |
| `data[].type` | `string` enum | Event subscription key |
| `data[].channel` | `string` enum | Delivery channel: `http` or `email` |
| `data[].meta` | `object` | Channel-specific configuration |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Credentials are missing, invalid, or do not belong to an API v1 organization |

```json
{
  "message": "Unauthorized"
}
```

## Example

```bash
curl "https://sdk.mediquo.com/v1/organizations/webhooks/" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -H "Accept: application/json"
```
