# PatientConsultationNoShowed

Fired when a patient does not attend a scheduled appointment.

## Event type

`patient_consultation_no_show`

## Payload structure

```json
{
  "type": "patient_consultation_no_show",
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
    "patient": {
      "code": "ORG_EXTERNAL_ID",
      "allergies": [],
      "diseases": [],
      "medications": []
    },
    "professional": {
      "id": "professional_hash",
      "name": "Dr. John Smith",
      "avatar": "https://..."
    }
  }
}
```

## Field reference

### `consultation`

See [Consultation](objects/consultation.md). Always an `appointment` type; `previous_scheduled_*` fields are always `null`.

### `patient`

See [Patient](objects/patient.md).

### `professional`

See [Professional](objects/professional.md).
