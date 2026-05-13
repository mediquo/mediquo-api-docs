# PatientPendingMessage

Fired when a patient has an unread message in a room.

## Event type

`patient_message_received`

## Payload structure

```json
{
  "type": "patient_message_received",
  "occurred_on": 1715000000,
  "payload": {
    "professional": {
      "id": "professional_hash",
      "name": "Dr. John Smith",
      "avatar": "https://..."
    },
    "patient": {
      "code": "ORG_EXTERNAL_ID"
    }
  }
}
```

## Field reference

### `professional`

See [Professional](objects/professional.md).

### `patient`

| Field | Type | Description |
|-------|------|-------------|
| `code` | `string` | Organization external patient identifier |

> Only the patient `code` is included in this payload (no allergies, diseases, or medications).
