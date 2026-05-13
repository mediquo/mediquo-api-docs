# PatientImmediateCallAssigned

Fired when an immediate video call is assigned to a professional.

## Event type

`patient_immediate_video_call_assigned`

## Payload structure

```json
{
  "type": "patient_immediate_video_call_assigned",
  "occurred_on": 1715000000,
  "payload": {
    "consultation": {
      "id": "uuid",
      "speciality": {
        "id": 1,
        "name": "General"
      },
      "url": "https://signed-video-url..."
    },
    "patient": {
      "code": "ORG_EXTERNAL_ID"
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

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Immediate call identifier |
| `speciality` | `object` | See [Speciality](objects/speciality.md) |
| `url` | `string` | Signed video room URL for the patient |

### `patient`

| Field | Type | Description |
|-------|------|-------------|
| `code` | `string` | Organization external patient identifier |

### `professional`

See [Professional](objects/professional.md).
