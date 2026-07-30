# Delete Organization Webhook

Stops an organization webhook subscription from receiving events. The operation
soft-deletes the subscription: it is excluded from future list responses and
event delivery, while its database record is retained.

## Request

**Method:** `DELETE`

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
| `Accept` | Recommended | Set to `application/json` |

### Body

This endpoint does not accept a request body.

## Responses

### `200 OK`

```json
{
  "message": "Organization webhook deleted"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `message` | `string` | Confirmation that the subscription was deleted |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Credentials are missing, invalid, or do not belong to an API v1 organization |
| `404 Not Found` | The subscription does not exist, is already deleted, or belongs to another organization |

```json
{
  "message": "Organization webhook not found with id: c0766228-bde8-447e-8284-040e79e952a8"
}
```

## Example

```bash
curl -X DELETE \
  "https://sdk.mediquo.com/v1/organizations/webhooks/<webhook-id>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -H "Accept: application/json"
```
