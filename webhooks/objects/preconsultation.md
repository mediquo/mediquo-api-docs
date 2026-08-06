# PreConsultation object

The reason the patient wrote and the documents they attached before the appointment.
Only populated for `appointment` consultations; `null`/`[]` when the patient did not fill in a
pre-consultation.

```json
{
  "reason": "Chest pain and shortness of breath since yesterday",
  "documents": [
    {
      "id": "uuid",
      "file_name": "report.pdf",
      "url": "https://..."
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `reason` | `string\|null` | Free text the patient wrote before the appointment. `null` if none |
| `documents` | `array` | Documents the patient attached. Empty array if none |
| `documents[].id` | `string` | Document identifier |
| `documents[].file_name` | `string` | Original file name |
| `documents[].url` | `string` | Signed, time-limited download URL. No authentication needed to download; expires 24h after the webhook is sent, so download it promptly |
