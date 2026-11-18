# Authenticate Patient

Authenticates a patient by their organization external ID and returns a JWT Bearer token to be used in patient-facing SDK calls.

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/patients/authenticate`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` from [Authenticate Organization](../authenticate/authenticate-organization.md) |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | `string` | Yes | Patient external ID within the organization |

## Responses

### `200 OK`

```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "token_type": "bearer"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `access_token` | `string` | JWT access token for the authenticated patient |
| `token_type` | `string` | Always `bearer` |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing or expired access token, invalid `x-api-key` or `x-secret-key`, the `code` field is missing, or the `code` does not match any patient in the organization |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/patients/authenticate" \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "code": "<patient-external-id>"
  }'
```
