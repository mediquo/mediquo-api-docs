# Get Immediate Video Call Schedule Availability

Returns the list of immediate video call schedules for the authenticated organization, including real-time availability (whether a professional is currently online and covering each schedule).

## Request

**Method:** `GET`
**URL:** `https://sdk-v2.mediquo.com/immediate-videocalls/schedules`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |

### Body

No request body.

## Responses

### `200 OK`

```json
{
  "data": [
    {
      "id": 1,
      "name": "General Medicine",
      "speciality_code": "general_medicine",
      "is_available": true,
      "schedule_description": "Available Mon–Fri 09:00–14:00"
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `data` | `array` | List of video call schedules for the organization |
| `data[].id` | `integer` | Unique identifier of the schedule |
| `data[].name` | `string` | Human-readable name of the schedule |
| `data[].speciality_code` | `string` | Code of the medical speciality associated to the schedule |
| `data[].is_available` | `boolean` | `true` when the schedule is active **and** at least one assigned professional is currently online |
| `data[].schedule_description` | `string` | Human-readable description of the schedule's active time windows |

Returns an empty `data` array if the organization has no video call schedules configured.

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing/invalid `x-api-key` / `x-secret-key` headers |

## Example

```bash
curl -X GET "https://sdk-v2.mediquo.com/immediate-videocalls/schedules" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>"
```
