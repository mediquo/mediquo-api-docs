# List Patient Appointments

Returns the paginated list of appointments of one of the organization's patients, identified by the patient code the organization assigned. No-show appointments and on-demand (immediate) video calls are excluded.

## Request

**Method:** `GET`
**URL:** `/patients/{patient_code}/appointments`

### Path parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `patient_code` | `string` | Yes | External identifier of the patient within the organization (the `patient.code` used when creating the patient or the appointment) |

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |

### Query parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `status` | `string` or `array` | No | Filter by appointment status. Accepts a single value (`status=pending`) or several (`status[]=pending&status[]=accepted`). One of `pending`, `expired`, `accepted`, `inCall`, `declined`, `cancelled`, `finished`, `free_of_charge`, `owed`, `unpaid`, `reporting` |
| `starts_at_gte` | `string` (date-time) | No | Only appointments starting at or after this date-time |
| `starts_at_lte` | `string` (date-time) | No | Only appointments starting at or before this date-time |
| `order[field]` | `string` | No | Field to sort by. Defaults to `starts_at` |
| `order[direction]` | `string` | No | `asc` or `desc`. Defaults to `asc` |
| `per_page` | `integer` | No | Results per page. Defaults to `15`, capped at `50`; values below `1` are raised to `1` |
| `page` | `integer` | No | Page number. Defaults to `1` |

### Body

No request body.

## Responses

### `200 OK`

```json
{
  "data": [
    {
      "appointment": {
        "code": "APPT-001",
        "channel": "videocall",
        "url": "https://mq.chat/a1b2c3",
        "starts_at": "2026-08-03T09:00:00+00:00",
        "duration": 1800,
        "status": "accepted"
      },
      "patient": {
        "code": "PATIENT-001"
      },
      "professional": {
        "hash": "5c9d7a12-9e44-4b0a-bb61-1d2f3e4a5b6c"
      },
      "services": [
        {
          "type": "first_visit",
          "name": "Primera visita"
        }
      ],
      "speciality": {
        "id": 12,
        "name": "General Medicine"
      }
    }
  ],
  "meta": {
    "current_page": 1,
    "last_page": 1,
    "per_page": 15,
    "total": 1
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data` | `array` | List of the patient's appointments |
| `data[].appointment.code` | `string\|null` | Appointment code in the organization's system |
| `data[].appointment.channel` | `string` | Channel of the appointment: `chat`, `videocall` or `phonecall` |
| `data[].appointment.url` | `string\|null` | Short URL to join the video call, `null` when the appointment has no video call |
| `data[].appointment.starts_at` | `string` (ISO 8601)`\|null` | Start date-time of the appointment |
| `data[].appointment.duration` | `integer` | Duration of the appointment in seconds |
| `data[].appointment.status` | `string` | Current status of the appointment (see the `status` filter for the full list) |
| `data[].patient.code` | `string\|null` | External identifier of the patient within the organization |
| `data[].professional.hash` | `string` (UUID)`\|null` | Professional hash, `null` if no professional is assigned yet |
| `data[].services` | `array` | Services booked for the appointment |
| `data[].services[].type` | `string` | Service code (e.g. `first_visit`, `successive_visit`, `emergency`) |
| `data[].services[].name` | `string` | Human-readable service name |
| `data[].speciality.id` | `integer` | Speciality identifier |
| `data[].speciality.name` | `string` | Speciality name |
| `meta.current_page` | `integer` | Current page number |
| `meta.last_page` | `integer` | Last available page number |
| `meta.per_page` | `integer` | Results per page |
| `meta.total` | `integer` | Total number of matching appointments |

Returns an empty `data` array if the patient has no appointments matching the filters.

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing `x-api-key` / `x-secret-key` headers |
| `403 Forbidden` | No patient with the given `patient_code` belongs to the authenticated organization |
| `422 Unprocessable Entity` | `status` contains a value that is not a valid appointment status |

## Example

```bash
curl -X GET "https://sdk-v2.mediquo.com/patients/PATIENT-001/appointments?status[]=pending&status[]=accepted&per_page=25&order[field]=starts_at&order[direction]=desc" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>"
```
