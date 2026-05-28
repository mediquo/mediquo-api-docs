# Create Appointment

Creates a new appointment between a professional and a patient. If an appointment with the same `code` already exists for the organization, it returns the existing one (idempotent).

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/appointments`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `appointment.code` | `string` | Yes | — | Unique identifier for the appointment in the organization's system |
| `appointment.starts_at` | `string` (ISO 8601) | Yes | — | Appointment start datetime. Must be present or future |
| `appointment.duration` | `integer` | No | `900` | Duration in seconds |
| `appointment.channel` | `string` | No | `videocall` | Communication channel: `videocall`, `chat`, `phonecall` |
| `professional.hash` | `string` | Yes | — | Hash identifier of the professional |
| `patient.code` | `string` | Yes | — | External identifier of the patient within the organization |
| `service.type` | `string` | No | `first_visit` | Type of visit: `first_visit`, `successive_visit`, `emergency` |
| `speciality.id` | `integer` | No | `1` | Speciality identifier |
| `services` | `array` | No | `[]` | List of additional services |
| `services[].type` | `string` | Yes (if `services` present) | — | Service type code |

## Responses

### `200 OK`

```json
{
  "data": {
    "appointment": {
      "code": "APPT-001",
      "url": "https://app.mediquo.com/join/abc123xyz"
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data.appointment.code` | `string` | The appointment code (matches the `appointment.code` sent in the request) |
| `data.appointment.url` | `string` | Short URL for the patient to join the appointment |

### Errors

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | Professional or patient not found, appointment already assigned to a different patient, or invalid room |
| `401 Unauthorized` | The `x-api-key` or `x-secret-key` headers are invalid |
| `403 Forbidden` | The patient's access has been blocked |
| `422 Unprocessable Entity` | Validation error (missing required fields, invalid date, unknown enum value) |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/appointments" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "appointment": {
      "code": "APPT-001",
      "starts_at": "2026-06-01T10:00:00Z",
      "duration": 900,
      "channel": "videocall"
    },
    "professional": {
      "hash": "abc123"
    },
    "patient": {
      "code": "PATIENT-001"
    },
    "service": {
      "type": "first_visit"
    },
    "speciality": {
      "id": 1
    }
  }'
```
