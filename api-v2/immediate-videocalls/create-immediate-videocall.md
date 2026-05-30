# Create Immediate Video Call

Creates an immediate (on-demand) video call for a patient. If the patient already has an active video call in the given schedule, the existing one is returned (idempotent).

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/immediate-videocalls`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — token obtained from [Authenticate Organization](../authenticate/authenticate-organization.md) |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `video_call_schedule_id` | `string` | Yes | — | ID of the video call schedule the patient is joining |
| `adapter` | `string` | No | `web` | Client adapter type: `web`, `cordova`, `flutter`, `native` |
| `patient.code` | `string` | Yes | — | External identifier of the patient within the organization |

## Responses

### `200 OK`

```json
{
  "data": {
    "id": "01j3k2m4n5p6q7r8s9t0",
    "organization_id": "b3f1c2d4-e5a6-7890-bcde-f12345678901",
    "room_id": null,
    "call_id": "550e8400-e29b-41d4-a716-446655440000",
    "from": null,
    "to": "a1b2c3d4e5f6",
    "to_contact_id": null,
    "consultation_url": "https://app.mediquo.com/join/abc123xyz",
    "status": "pending"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data.id` | `string` | Unique identifier of the immediate video call |
| `data.organization_id` | `string` (UUID) | ID of the organization |
| `data.room_id` | `integer\|null` | Room identifier, populated once a professional is assigned |
| `data.call_id` | `string\|null` | UUID of the underlying call session |
| `data.from` | `string\|null` | Hash of the assigned professional; `null` while still pending |
| `data.to` | `string` | Hash of the patient |
| `data.to_contact_id` | `string\|null` | Contact ID of the assigned professional; `null` while still pending |
| `data.consultation_url` | `string\|null` | URL the patient uses to join the video call |
| `data.status` | `string` | Current status: `pending`, `accepted`, `reporting`, `finished`, `declined`, `expired` |

### Errors

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | Patient not found or the specified `video_call_schedule_id` does not exist |
| `401 Unauthorized` | Missing or invalid `Authorization` Bearer token, `x-api-key`, or `x-secret-key` |
| `403 Forbidden` | The patient's access has been blocked |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/immediate-videocalls" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -H "Content-Type: application/json" \
  -d '{
    "video_call_schedule_id": "sched-abc123",
    "adapter": "web",
    "patient": {
      "code": "PATIENT-001"
    }
  }'
```
