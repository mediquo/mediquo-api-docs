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
      "assessment": "J06.9 Acute upper respiratory infection, unspecified",
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
| `content.assessment` | `string\|null` | Assessment / analysis section (A). When the professional selected a diagnosis, this carries the ICD-10 (CIE-10) code and its description — see below |
| `content.plan` | `string\|null` | Plan section (P) |
| `patient.code` | `string\|null` | Organization external patient identifier |

## ICD-10 (CIE-10) diagnosis in `assessment`

The assessment section is not free text: it is built from the diagnosis the
professional selected for the report, formatted as the ICD-10 code followed by a
space and the diagnosis description:

```
<code> <description>
```

For example, `"J06.9 Acute upper respiratory infection, unspecified"` — the code
is `J06.9` and the rest is the description. To extract the code, take the first
whitespace-delimited token.

The description is stored in the language of the imported ICD-10 catalogue, so
it is not necessarily English.

If the professional did not select a diagnosis, `content.assessment` is `null`.
The code is not delivered as a separate field in this webhook.

## Delivery target

Delivered to the organization that owns the report
(`reports.organization_id`). If the report has no organization,
no webhook is sent.
