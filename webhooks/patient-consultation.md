# PatientConsultation

Fired for the full lifecycle of a consultation (appointment or chat).

## Event types

| Type | Trigger |
|------|---------|
| `patient_consultation_created` | Appointment booked |
| `patient_consultation_cancelled` | Appointment cancelled |
| `patient_consultation_rescheduled` | Appointment date or professional changed |
| `patient_consultation_finished` | Appointment or chat consultation finished |

## Payload structure

```json
{
  "type": "patient_consultation_created",
  "occurred_on": 1715000000,
  "payload": {
    "consultation": {
      "id": "uuid",
      "type": "appointment",
      "channel": "video",
      "service_type": "scheduled",
      "consultation_url": "https://...",
      "scheduled_start_date": "2024-05-01T10:00:00+00:00",
      "scheduled_end_date": "2024-05-01T10:15:00+00:00",
      "previous_scheduled_start_date": null,
      "previous_scheduled_end_date": null,
      "start_date": null,
      "end_date": null,
      "created_at": "2024-04-30T09:00:00+00:00",
      "speciality": {
        "id": 1,
        "name": "General"
      }
    },
    "reports": [
      {
        "id": "uuid",
        "type": "soap",
        "content": {
          "subjective": "Patient description...",
          "objective": "Clinical findings...",
          "assessment": "Diagnosis...",
          "plan": "Treatment plan..."
        }
      }
    ],
    "patient": {
      "code": "ORG_EXTERNAL_ID",
      "allergies": [
        { "id": 1, "name": "Penicillin", "description": null }
      ],
      "diseases": [
        { "id": 2, "name": "Hypertension", "description": null }
      ],
      "medications": [
        { "id": 3, "name": "Enalapril", "description": null, "posology": "10mg/day" }
      ]
    },
    "professional": {
      "id": "professional_hash",
      "name": "Dr. John Smith",
      "avatar": "https://..."
    },
    "prescriptions": [
      {
        "id": "uuid",
        "active_substances": ["paracetamol"]
      }
    ],
    "derivation": null
  }
}
```

## Field reference

### `consultation`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Unique consultation identifier |
| `type` | `string` | `appointment` or `chat` |
| `channel` | `string` | `video`, `phone`, `chat` |
| `service_type` | `string\|null` | `scheduled`, `immediate`, etc. |
| `consultation_url` | `string\|null` | Short URL for the video room |
| `scheduled_start_date` | `ISO8601\|null` | Planned start |
| `scheduled_end_date` | `ISO8601\|null` | Planned end |
| `previous_scheduled_start_date` | `ISO8601\|null` | Only present on `rescheduled` events |
| `previous_scheduled_end_date` | `ISO8601\|null` | Only present on `rescheduled` events |
| `start_date` | `ISO8601\|null` | Actual start |
| `end_date` | `ISO8601\|null` | Actual end |
| `created_at` | `ISO8601` | Creation timestamp |
| `speciality` | `object` | See [Speciality](objects/speciality.md) |

### `patient`

See [Patient](objects/patient.md).

### `professional`

See [Professional](objects/professional.md).

### `reports`

Array of [Report](objects/report.md) objects. Empty array if no report exists.

### `prescriptions`

Array of [Prescription](objects/prescription.md) objects. Empty array if none.

### `derivation`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Derivation identifier |
| `description` | `string` | Derivation description |

`null` when there is no derivation. Only populated on `patient_consultation_finished` for appointments.
