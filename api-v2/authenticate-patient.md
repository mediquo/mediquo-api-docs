# Authenticate Patient

Authenticates a patient by their organization external ID and returns a JWT Bearer token to be used in patient-facing SDK calls.

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/patients/authenticate`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | Organization API key |
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
| `401 Unauthorized` | The `code` does not match any patient in the organization, or the `x-api-key` is invalid |
| `422 Unprocessable Entity` | The `code` field is missing from the request body |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/patients/authenticate" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -d '{
    "code": "<patient-external-id>"
  }'
```
