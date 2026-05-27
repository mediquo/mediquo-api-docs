# TriagePositionUpdated

Fired when an immediate video call is assigned or declined, recalculating
the waiting-room position of the patients still queued behind it.

The subscription type is `triage_position_updated`, but the delivered
envelope `type` is `patient_waiting_room_position_update`. One webhook is
sent per patient whose position changed.

## Event types

| Type | Trigger |
|------|---------|
| `patient_waiting_room_position_update` | Immediate video call assigned or declined — `occurred_on` is the event timestamp |

## Payload structure

```json
{
  "type": "patient_waiting_room_position_update",
  "occurred_on": 1715000000,
  "payload": {
    "consultation": {
      "speciality": {
        "id": "speciality-id",
        "name": "Schedule name"
      },
      "position": 3,
      "url": "https://.../consultation?token=..."
    },
    "patient": {
      "code": "ORG_EXTERNAL_ID"
    }
  }
}
```

## Field reference

| Field | Type | Description |
|-------|------|-------------|
| `consultation.speciality.id` | `string\|null` | Speciality identifier |
| `consultation.speciality.name` | `string\|null` | Schedule name for the speciality |
| `consultation.position` | `integer` | Patient position in the waiting room |
| `consultation.url` | `string\|null` | Signed consultation URL for the patient |
| `patient.code` | `string\|null` | Organization external patient identifier |

## Delivery target

Delivered to the organization that owns the immediate video call
(`immediate_video_calls.organization_id`), for every patient queued
after the assigned/declined call.
