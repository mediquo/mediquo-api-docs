# Report object

```json
{
  "id": "uuid",
  "type": "soap",
  "content": {
    "subjective": "Patient describes...",
    "objective": "Clinical findings...",
    "assessment": "Diagnosis...",
    "plan": "Treatment plan..."
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Report UUID |
| `type` | `string` | Always `soap` |
| `content.subjective` | `string\|null` | Patient's own description (anamnesis) |
| `content.objective` | `string\|null` | Clinical findings |
| `content.assessment` | `string\|null` | Diagnosis or assessment |
| `content.plan` | `string\|null` | Treatment plan |
