# Update Organization Webhook

Replaces the `type`, `channel`, and `meta` of an existing subscription owned by
the authenticated organization. The subscription `id` does not change.

See [Organization Webhooks](README.md) for all subscription types, their
delivered payload types, supported channels, and metadata.

## Request

**Method:** `PUT`

**URL:** `https://sdk.mediquo.com/v1/organizations/webhooks/{webhookId}`

### Path parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `webhookId` | `string` (UUID v4) | Identifier returned when the subscription was created or listed |

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | API key of an API v1 organization |
| `x-secret-key` | Yes | Secret key of the same organization |
| `Content-Type` | Yes | Must be `application/json` |
| `Accept` | Recommended | Set to `application/json` |

### Body

This is a full replacement, so all three top-level fields are required.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | `string` enum | Yes | Event subscription key listed in the [overview](README.md#subscription-types) |
| `channel` | `string` enum | Yes | `http` or `email` |
| `meta` | `object` | Yes | Channel-specific configuration |
| `meta.url` | `string` (URL) | For `channel=http` | Destination URL for outgoing webhook requests |
| `meta.headers` | `object` | No | Custom headers added to outgoing HTTP requests |

```json
{
  "type": "appointment_created",
  "channel": "http",
  "meta": {
    "url": "https://client.example.com/hooks/appointments"
  }
}
```

## Responses

### `200 OK`

```json
{
  "data": {
    "id": "c0766228-bde8-447e-8284-040e79e952a8",
    "type": "appointment_created",
    "channel": "http",
    "meta": {
      "url": "https://client.example.com/hooks/appointments"
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data.id` | `string` (UUID v4) | Existing subscription identifier |
| `data.type` | `string` enum | Updated event subscription key |
| `data.channel` | `string` enum | Updated delivery channel |
| `data.meta` | `object` | Updated channel-specific configuration |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Credentials are missing, invalid, or do not belong to an API v1 organization |
| `404 Not Found` | The subscription does not exist, is deleted, or belongs to another organization |
| `422 Unprocessable Entity` | A required field is missing, an enum value is unknown, or `meta.url` is missing or invalid for the `http` channel |

```json
{
  "message": "Organization webhook not found with id: c0766228-bde8-447e-8284-040e79e952a8"
}
```

## Example

```bash
curl -X PUT \
  "https://sdk.mediquo.com/v1/organizations/webhooks/<webhook-id>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "type": "appointment_created",
    "channel": "http",
    "meta": {
      "url": "https://client.example.com/hooks/appointments"
    }
  }'
```
