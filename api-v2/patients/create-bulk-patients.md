# Create Bulk Patients

Creates up to 100 patients in a single request. The operation is processed asynchronously — patients are queued for creation and the endpoint returns immediately.

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/patients/bulk`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `patients` | `array` | Yes | List of patients to create. Maximum 100 items |
| `patients.*.first_name` | `string` | Yes | Patient first name |
| `patients.*.code` | `string` | Yes | Patient external ID within the organization |
| `patients.*.last_name` | `string` | No | Patient last name |
| `patients.*.tax_id` | `string` | No | Patient tax / national ID number |
| `patients.*.phone` | `string` | No | Patient phone number |
| `patients.*.phone_prefix` | `string` | No | Phone country prefix (e.g. `+34`) |
| `patients.*.gender` | `string` | No | Patient gender. Accepted values: `male`, `female` |
| `patients.*.birth_date` | `string` | No | Date of birth in `YYYY-MM-DD` format |
| `patients.*.email` | `string` | No | Patient email address |
| `patients.*.plan` | `string` | No | Plan or product identifier assigned to the patient |
| `patients.*.locale` | `string` | No | Patient preferred locale (e.g. `es`, `en`) |
| `patients.*.meta` | `object` | No | Arbitrary key-value metadata |

## Responses

### `200 OK`

```json
{
  "message": "Success"
}
```

The response confirms the request was accepted. Patient creation happens asynchronously in the background.

### Errors

| Status | Condition |
|--------|-----------|
| `400 Bad Request` | The `patients` field is missing from the request body |
| `401 Unauthorized` | The `x-api-key` or `x-secret-key` headers are invalid |
| `422 Unprocessable Entity` | One or more patient entries fail validation (e.g. missing `first_name` or `code`, invalid `gender`, wrong `birth_date` format, or more than 100 patients) |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/patients/bulk" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "patients": [
      {
        "first_name": "Jane",
        "last_name": "Doe",
        "code": "patient-001",
        "email": "jane.doe@example.com",
        "gender": "female",
        "birth_date": "1990-05-15",
        "phone": "612345678",
        "phone_prefix": "+34",
        "locale": "es"
      },
      {
        "first_name": "John",
        "code": "patient-002"
      }
    ]
  }'
```
