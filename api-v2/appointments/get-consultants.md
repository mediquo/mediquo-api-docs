# Get Appointment Consultants

Returns the list of professionals (appointment consultants) enabled to receive appointments for the authenticated organization.

## Request

**Method:** `GET`
**URL:** `/appointments/consultants`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |

### Body

No request body. This endpoint accepts no query parameters.

## Responses

### `200 OK`

```json
{
  "data": [
    {
      "id": "5c9d7a12-9e44-4b0a-bb61-1d2f3e4a5b6c",
      "name": "Dr. Ana García",
      "collegiate_number": "280012345",
      "tax_id": "12345678Z",
      "avatar": "https://cdn.mediquo.com/avatars/5c9d7a12.jpg",
      "speciality": {
        "id": 12
      }
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data` | `array` | List of the organization's appointment consultants |
| `data[].id` | `string` (UUID) | Professional hash — use it as `professional_id` in [Get Schedule Availability](get-schedule-availability.md) |
| `data[].name` | `string` | Professional's name |
| `data[].collegiate_number` | `string\|null` | Professional's medical collegiate number, `null` if not set |
| `data[].tax_id` | `string\|null` | Professional's billing tax identifier, `null` if not set |
| `data[].avatar` | `string` | URL of the professional's avatar image |
| `data[].speciality` | `object` | Speciality assigned to the consultant within the organization |
| `data[].speciality.id` | `integer` | Speciality identifier |

Returns an empty `data` array if the organization has no appointment consultants configured.

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing/invalid `x-api-key` / `x-secret-key` headers |

## Example

```bash
curl -X GET "https://sdk-v2.mediquo.com/appointments/consultants" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>"
```
