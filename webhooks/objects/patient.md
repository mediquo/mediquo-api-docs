# Patient object

```json
{
  "code": "ORG_EXTERNAL_ID",
  "allergies": [
    { "id": 1, "name": "Penicillin", "description": null }
  ],
  "diseases": [
    { "id": 2, "name": "Hypertension", "description": null }
  ],
  "medications": [
    { "id": 3, "name": "Enalapril", "description": null, "posology": "10mg/day" }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `code` | `string` | Organization external patient identifier |
| `allergies` | `array` | Patient allergies |
| `allergies[].id` | `integer` | Allergy identifier |
| `allergies[].name` | `string` | Allergy name |
| `allergies[].description` | `string\|null` | Additional description |
| `diseases` | `array` | Chronic diseases |
| `diseases[].id` | `integer` | Disease identifier |
| `diseases[].name` | `string` | Disease name |
| `diseases[].description` | `string\|null` | Additional description |
| `medications` | `array` | Current medications |
| `medications[].id` | `integer` | Medication identifier |
| `medications[].name` | `string` | Medication name |
| `medications[].description` | `string\|null` | Additional description |
| `medications[].posology` | `string\|null` | Dosage instructions |
