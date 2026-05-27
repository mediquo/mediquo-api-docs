# Create Appointment

Creates a new appointment between a professional and a patient. If an appointment with the same `code` already exists for the organization, it returns the existing one (idempotent).

## Request

**Method:** `POST`
**URL:** `/appointments`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | ✅ | `Bearer <access_token>` obtained from [Authenticate Organization](authenticate-organization.md) |
| `Content-Type` | ✅ | `application/json` |

### Body

```json
{
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
  },
  "services": [
    { "type": "ecg" }
  ]
}
```

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `appointment.code` | `string` | ✅ | — | Unique identifier for the appointment in the organization's system |
| `appointment.starts_at` | `string` (ISO 8601) | ✅ | — | Appointment start datetime. Must be present or future |
| `appointment.duration` | `integer` | ❌ | `900` | Duration in seconds |
| `appointment.channel` | `string` | ❌ | `videocall` | Communication channel: `videocall`, `chat`, `phonecall` |
| `professional.hash` | `string` | ✅ | — | Hash identifier of the professional |
| `patient.code` | `string` | ✅ | — | External identifier of the patient within the organization |
| `service.type` | `string` | ❌ | `first_visit` | Type of visit: `first_visit`, `successive_visit`, `emergency` |
| `speciality.id` | `integer` | ❌ | `1` | Speciality identifier |
| `services` | `array` | ❌ | `[]` | List of additional services |
| `services[].type` | `string` | ✅ (if `services` present) | — | Service type code |

## Responses

### `200 OK`

Returned when the appointment is successfully created or already exists.

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

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | Professional or patient not found, appointment already assigned to a different patient, or invalid room |
| `401 Unauthorized` | Missing or invalid Bearer token |
| `403 Forbidden` | The patient's access has been blocked |
| `422 Unprocessable Entity` | Validation error (missing required fields, invalid date, unknown enum value) |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/appointments" \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
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
