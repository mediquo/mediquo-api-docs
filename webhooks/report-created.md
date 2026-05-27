# ReportCreated

Fired when a professional generates a SOAP report for a patient.

The subscription type is `report_created`, but the delivered envelope
`type` is `patient_report_sent`.

## Event types

| Type | Trigger |
|------|---------|
| `patient_report_sent` | Report generated — `occurred_on` is the report `created_at` |

## Payload structure

```json
{
  "type": "patient_report_sent",
  "occurred_on": 1715000000,
  "payload": {
    "type": "soap",
    "content": {
      "subjective": "...",
      "objective": "...",
      "assessment": "...",
      "plan": "..."
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
| `payload.type` | `string` | Report format, always `soap` |
| `content.subjective` | `string\|null` | Subjective section (S) |
| `content.objective` | `string\|null` | Objective section (O) |
| `content.assessment` | `string\|null` | Assessment / analysis section (A) |
| `content.plan` | `string\|null` | Plan section (P) |
| `patient.code` | `string\|null` | Organization external patient identifier |

## Delivery target

Delivered to the organization that owns the report
(`reports.organization_id`). If the report has no organization,
no webhook is sent.
