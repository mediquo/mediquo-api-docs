# Cancel Appointment

Cancels an appointment of the authenticated organization, identified by the `code` the organization assigned when creating it (see [Create Appointment](create-appointment.md)).

## Request

**Method:** `DELETE`
**URL:** `/appointments/{code}`

### Path parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | `string` | Yes | The appointment code in the organization's system (the `appointment.code` sent to [Create Appointment](create-appointment.md)) |

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |

### Body

The body is optional. Send it as JSON with `Content-Type: application/json`.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `cancel_reason` | `string` | No | Free-text reason for the cancellation. Max 255 characters. Stored on the cancelled appointment |

```json
{
  "cancel_reason": "Patient cannot attend"
}
```

`cancel_reason` can also be sent as a query parameter (`/appointments/{code}?cancel_reason=...`) for clients that cannot send a body with `DELETE` requests.

## Responses

### `200 OK`

The appointment status is set to `cancelled` and its no-show flag is cleared. If `cancel_reason` was sent, it is stored on the appointment. The response body is empty.

```json
{}
```

Cancelling an appointment that is already cancelled succeeds and returns the same response.

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | No appointment with the given `code` exists for the authenticated organization |
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing/invalid `x-api-key` / `x-secret-key` headers |
| `422 Unprocessable Entity` | `cancel_reason` is not a string or exceeds 255 characters. The appointment is not cancelled |

Error responses carry a `message` field describing the failure:

```json
{
  "message": "Appointment not found by provider external id: APPT-001"
}
```

## Example

```bash
curl -X DELETE "https://sdk-v2.mediquo.com/appointments/APPT-001" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>"
```

With a cancellation reason:

```bash
curl -X DELETE "https://sdk-v2.mediquo.com/appointments/APPT-001" \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{"cancel_reason": "Patient cannot attend"}'
```
