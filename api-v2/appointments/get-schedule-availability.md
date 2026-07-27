# Get Schedule Availability

Returns the paginated list of bookable appointment slots for the authenticated organization, ordered by start date. Only slots that are `available` and that still satisfy the schedule's minimum booking notice are returned.

## Request

**Method:** `GET`
**URL:** `/appointments/schedule-availability`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |

### Query parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `professional_id` | `string` (UUID) | No | Filter slots by professional hash |
| `speciality_id` | `integer` | No | Filter slots by the professional's speciality identifier |
| `channel` | `string` | No | Filter slots by channel. One of `chat`, `videocall`, `phonecall` |
| `services` | `string` | No | Comma-separated list of service codes (e.g. `first_visit,successive_visit`). A slot is returned only if it offers **all** the requested services |
| `starts_at_gte` | `string` (date-time) | No | Only slots starting at or after this date-time |
| `starts_at_lte` | `string` (date-time) | No | Only slots starting at or before this date-time |
| `page` | `integer` | No | Page number. Page size is fixed at 500 |

### Body

No request body.

## Responses

### `200 OK`

```json
{
  "data": [
    {
      "id": "9b1f4d5e-2c3a-4f18-9d77-8a4c1e5b2f30",
      "channel": "videocall",
      "starts_at": "2026-08-03T09:00:00+00:00",
      "duration": 1800,
      "finishes_at": "2026-08-03T09:30:00+00:00",
      "status": "available",
      "price": 3500,
      "currency": "EUR",
      "professional": {
        "id": "5c9d7a12-9e44-4b0a-bb61-1d2f3e4a5b6c",
        "name": "Dr. Ana García"
      },
      "speciality": {
        "id": 12
      },
      "services": [
        {
          "type": "first_visit",
          "name": "Primera visita"
        }
      ]
    }
  ],
  "meta": {
    "current_page": 1,
    "last_page": 1,
    "per_page": 500,
    "total": 1
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data` | `array` | List of bookable slots, ordered by `starts_at` ascending |
| `data[].id` | `string` | Unique identifier of the slot |
| `data[].channel` | `string` | Channel of the slot: `chat`, `videocall` or `phonecall` |
| `data[].starts_at` | `string` (ISO 8601) | Start date-time of the slot |
| `data[].duration` | `integer` | Duration of the slot in seconds |
| `data[].finishes_at` | `string` (ISO 8601) | End date-time of the slot |
| `data[].status` | `string` | Status of the slot. Always `available` in this endpoint |
| `data[].price` | `integer` | Price of the slot in the currency's minor units (cents) |
| `data[].currency` | `string` | Currency code of the price (e.g. `EUR`, `USD`, `ARS`) |
| `data[].professional` | `object` | Professional owning the slot |
| `data[].professional.id` | `string` (UUID) | Professional hash |
| `data[].professional.name` | `string` | Professional's name |
| `data[].speciality` | `object` | Speciality of the professional |
| `data[].speciality.id` | `integer\|null` | Speciality identifier, `null` if the professional has none |
| `data[].services` | `array` | Services offered on this slot |
| `data[].services[].type` | `string` | Service code (e.g. `first_visit`, `successive_visit`, `emergency`) |
| `data[].services[].name` | `string` | Human-readable service name |
| `meta.current_page` | `integer` | Current page number |
| `meta.last_page` | `integer` | Last available page number |
| `meta.per_page` | `integer` | Slots per page (fixed at 500) |
| `meta.total` | `integer` | Total number of matching slots |

Returns an empty `data` array if no slot matches the filters.

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | `channel` is not one of `chat`, `videocall`, `phonecall` |
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing/invalid `x-api-key` / `x-secret-key` headers |

## Example

```bash
curl -X GET "https://sdk-v2.mediquo.com/appointments/schedule-availability?channel=videocall&services=first_visit&starts_at_gte=2026-08-01T00:00:00Z&starts_at_lte=2026-08-31T23:59:59Z" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>"
```
