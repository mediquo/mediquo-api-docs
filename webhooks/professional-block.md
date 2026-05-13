# ProfessionalBlock

Fired when a professional availability block is created, updated, or deleted.

## Event types

| Type | Trigger |
|------|---------|
| `professional_block_created` | Block created — `occurred_on` is `created_at` |
| `professional_block_updated` | Block modified — `occurred_on` is `updated_at` |
| `professional_block_deleted` | Block deleted — `occurred_on` is `deleted_at` |

## Payload structure

```json
{
  "type": "professional_block_created",
  "occurred_on": 1715000000,
  "payload": {
    "id": "block-uuid",
    "name": "Vacation",
    "starts_at": "2024-06-01T08:00:00+00:00",
    "finishes_at": "2024-06-07T20:00:00+00:00",
    "professional": {
      "hash": "professional_hash",
      "name": "Dr. John Smith",
      "speciality": "General"
    }
  }
}
```

## Field reference

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Block identifier |
| `name` | `string` | Block label |
| `starts_at` | `ISO8601` | Block start |
| `finishes_at` | `ISO8601` | Block end |
| `professional.hash` | `string` | Professional identifier |
| `professional.name` | `string` | Professional display name |
| `professional.speciality` | `string\|null` | Professional speciality name, `null` if not set |
