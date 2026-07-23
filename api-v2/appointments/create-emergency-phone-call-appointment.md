# Create Emergency Phone Call Appointment

Creates an emergency phone call appointment for a patient. It finds or creates the patient, assigns an available general medicine professional, creates the contact, and schedules the appointment with a phone call flow.

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/appointments/emergency`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `appointment.code` | `string` | Yes | Unique identifier for the appointment in the organization's system |
| `appointment.origin` | `string` | No | External origin identifier |
| `appointment.channel` | `string` | Yes | Communication channel. Values: `chat`, `videocall`, `phone_call` |
| `appointment.type` | `string` | Yes | Service type. Values: `first_visit`, `follow_up`, `emergency`, `phone_call` |
| `appointment.description` | `string` | No | Appointment description |
| `patient.code` | `string` | Yes | External identifier of the patient within the organization |
| `patient.first_name` | `string` | Yes | Patient's first name |
| `patient.last_name` | `string` | Yes | Patient's last name |
| `patient.tax_id` | `string` | No | Tax ID (DNI/NIE/Passport) |
| `patient.phone_prefix` | `string` | Yes | Phone country prefix (e.g. `34`) |
| `patient.phone` | `string` | Yes | Phone number |
| `patient.birthdate` | `string` | No | Date of birth in `Y-m-d` format (e.g. `1990-01-15`) |
| `patient.gender` | `string` | No | Gender. Values: `male`, `female`, `other` |

## Responses

### `200 OK`

```json
{
    "id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` (UUID) | The created appointment identifier |

### Errors

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | Validation error or unexpected error |
| `403 Forbidden` | The patient's access has been blocked (banned) |
| `404 Not Found` | No professional available or resource not found |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/appointments/emergency" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "appointment": {
        "code": "EXT-001",
        "channel": "phone_call",
        "type": "emergency"
    },
    "patient": {
        "code": "PAT-001",
        "first_name": "Juan",
        "last_name": "Pérez",
        "phone_prefix": "34",
        "phone": "612345678"
    }
}'
```
