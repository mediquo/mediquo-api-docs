# Authenticate Organization

Obtains a Bearer access token for an organization. The token is valid for **10 minutes** and must be used in subsequent API v2 calls.

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/authenticate`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `username` | `string` | Yes | Organization ID |
| `password` | `string` | Yes | Organization password (`api_password`) |

## Responses

### `200 OK`

```json
{
  "token_type": "Bearer",
  "access_token": "2|abc123..."
}
```

| Field | Type | Description |
|-------|------|-------------|
| `token_type` | `string` | Always `Bearer` |
| `access_token` | `string` | Access token. Expires after 10 minutes |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Invalid credentials (`username`, `password`, `x-api-key` or `x-secret-key` do not match) or the organization does not have API version `v2` enabled |
| `422 Unprocessable Entity` | Missing `username` or `password` fields in the request body |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/authenticate" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "username": "<organization-id>",
    "password": "<api-password>"
  }'
```

## Notes

- The generated token includes the `organization-api_version-v2` ability, required by all API v2 endpoints.
- Use the received `access_token` as `Authorization: Bearer <token>` header in subsequent requests.
