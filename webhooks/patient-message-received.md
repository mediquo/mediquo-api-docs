# PatientMessageReceived

Fired when a professional sends a message to a patient.

The webhook is dispatched with a **2 second delay** so the patient has a short window to read the message before the integrator is notified. The `message.is_read` flag reflects the actual message status at delivery time (i.e. after the delay), so a webhook may arrive with `is_read=true` if the patient opened the conversation within those 2 seconds.

The integrator is responsible for deciding what to do with the event based on the read state and any previous notifications already sent to the patient.

## Subscription

Configured per organization in the `organization_webhooks` table:

| Field | Value |
|-------|-------|
| `type` | `patient_message_received` |
| `channel` | `http` |
| `meta.url` | Target URL |
| `meta.headers` | Optional extra HTTP headers |

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
    },
    "message": {
      "is_read": false
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

### `message`

| Field | Type | Description |
|-------|------|-------------|
| `is_read` | `bool` | Whether the patient has already read the message at the moment the webhook is delivered (after the 2s delay) |

> Only the patient `code` is included in this payload (no allergies, diseases, or medications).
