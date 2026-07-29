# Create Patient

Creates a single patient for the authenticated organization. If a patient with the same `code` already exists in the organization, its data is updated instead of creating a duplicate — the patient keeps its identity. For creating many patients at once, use [Create Bulk Patients](create-bulk-patients.md).

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/patients`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `application/json` |

### Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `first_name` | `string` | Yes | Patient first name |
| `code` | `string` | Yes | Patient external ID within the organization. Used as the patient identifier in the rest of the API (e.g. [List Patient Appointments](list-patient-appointments.md)) |
| `last_name` | `string` | No | Patient last name |
| `tax_id` | `string` | No | Patient tax / national ID number |
| `gender` | `string` | No | Patient gender. Accepted values: `male`, `female` |
| `birth_date` | `string` | No | Date of birth in `YYYY-MM-DD` format |
| `email` | `string` | No | Patient email address |
| `plan` | `string` | No | Plan or product identifier assigned to the patient |
| `locale` | `string` | No | Patient preferred locale. Recognised values: `es`, `en`, `pt`, `de`, `ca`. Any other value falls back to `es` |
| `phone` | `string` | No | Patient phone number |
| `phone_prefix` | `string` | No | Phone country prefix (e.g. `+34`). If omitted **and** no `phone` is sent, it defaults to the prefix of the organization's country |
| `meta` | `object` | No | Arbitrary key-value metadata |

The patient's country is taken from the organization, not from the request.

## Responses

### `200 OK`

```json
{
  "message": "Success"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `message` | `string` | Always `Success`. The patient was created or updated |

The response does not return the created patient. To read it back, use the patient retrieval endpoint with the `code` you sent.

### `4xx` / `5xx`

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing, invalid, or expired Bearer token; or missing/invalid `x-api-key` / `x-secret-key` headers |
| `422 Unprocessable Entity` | Validation failed: `first_name` or `code` missing, `gender` not in `male`/`female`, `birth_date` not in `YYYY-MM-DD` format, `email` not a valid address, or `meta` not an object |

The `422` body carries the field errors keyed by field name:

```json
{
  "message": {
    "first_name": ["The first name field is required."],
    "gender": ["The selected gender is invalid."]
  }
}
```

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/patients" \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -d '{
    "first_name": "Jane",
    "last_name": "Doe",
    "code": "patient-001",
    "tax_id": "12345678Z",
    "email": "jane.doe@example.com",
    "gender": "female",
    "birth_date": "1990-05-15",
    "phone": "612345678",
    "phone_prefix": "+34",
    "locale": "es",
    "meta": {
      "policy_number": "POL-9988"
    }
  }'
```
