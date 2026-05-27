# PrescriptionCreated

Fired when a prescription PDF has been generated and is ready for download.

The webhook is bound to the `PrescriptionPDFGenerated` event so that
`pdf_url` is populated when the webhook is delivered.

## Event types

| Type | Trigger |
|------|---------|
| `prescription_created` | Prescription PDF generated — `occurred_on` is the prescription `created_at` |

## Payload structure

```json
{
  "type": "prescription_created",
  "occurred_on": 1715000000,
  "payload": {
    "prescription": {
      "id": "prescription-uuid",
      "short_code": "ABC123",
      "active_substances": ["paracetamol", "ibuprofen"],
      "prescribed_at": "2026-05-21T10:30:00+00:00",
      "pdf_url": "https://.../prescription-uuid.pdf",
      "consultation_type": "chat",
      "consultation_id": "consultation-uuid"
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

| Field | Type | Description |
|-------|------|-------------|
| `prescription.id` | `string` | Prescription UUID |
| `prescription.short_code` | `string\|null` | Short code for the prescription |
| `prescription.active_substances` | `array` | List of active substance entries |
| `prescription.prescribed_at` | `ISO8601` | Date the prescription was prescribed |
| `prescription.pdf_url` | `string\|null` | Public URL of the generated PDF |
| `prescription.consultation_type` | `string\|null` | Originating consultation type (`chat`, `appointment`, etc.) |
| `prescription.consultation_id` | `string\|null` | Originating consultation identifier |
| `patient.code` | `string\|null` | Organization external patient identifier |
| `professional.id` | `string` | Professional hash identifier |
| `professional.name` | `string` | Professional display name |
| `professional.avatar` | `string\|null` | Professional avatar URL |

## Delivery target

The webhook is delivered to the organization that owns the patient
(`patients.organization_id`). If the prescription has no patient,
no webhook is sent.
