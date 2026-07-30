# Create Organization Webhook

Creates an outgoing webhook subscription for the authenticated organization.

See [Organization Webhooks](README.md) for all subscription types, their
delivered payload types, supported channels, and metadata.

## Request

**Method:** `POST`

**URL:** `https://sdk.mediquo.com/v1/organizations/webhooks/`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | API key of an API v1 organization |
| `x-secret-key` | Yes | Secret key of the same organization |
| `Content-Type` | Yes | Must be `application/json` |
| `Accept` | Recommended | Set to `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | `string` enum | Yes | Event subscription key listed in the [overview](README.md#subscription-types) |
| `channel` | `string` enum | Yes | `http` or `email` |
| `meta` | `object` | Yes | Channel-specific configuration |
| `meta.url` | `string` (URL) | For `channel=http` | Destination URL for outgoing webhook requests |
| `meta.headers` | `object` | No | Custom headers added to outgoing HTTP requests |

```json
{
  "type": "report_created",
  "channel": "http",
  "meta": {
    "url": "https://client.example.com/hooks/report",
    "headers": {
      "X-Tenant": "acme"
    }
  }
}
```

## Responses

### `201 Created`

```json
{
  "data": {
    "id": "c0766228-bde8-447e-8284-040e79e952a8",
    "type": "report_created",
    "channel": "http",
    "meta": {
      "url": "https://client.example.com/hooks/report",
      "headers": {
        "X-Tenant": "acme"
      }
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data.id` | `string` (UUID v4) | Server-generated subscription identifier |
| `data.type` | `string` enum | Event subscription key |
| `data.channel` | `string` enum | Delivery channel |
| `data.meta` | `object` | Stored channel-specific configuration |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Credentials are missing, invalid, or do not belong to an API v1 organization |
| `422 Unprocessable Entity` | A required field is missing, an enum value is unknown, or `meta.url` is missing or invalid for the `http` channel |

Example validation response:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "meta.url": [
      "The meta.url field is required when channel is http."
    ]
  }
}
```

## Example

```bash
curl -X POST "https://sdk.mediquo.com/v1/organizations/webhooks/" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "type": "report_created",
    "channel": "http",
    "meta": {
      "url": "https://client.example.com/hooks/report"
    }
  }'
```
