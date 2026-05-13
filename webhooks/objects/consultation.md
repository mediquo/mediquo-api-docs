# Consultation object

```json
{
  "id": "uuid",
  "type": "appointment",
  "channel": "video",
  "service_type": "scheduled",
  "consultation_url": "https://...",
  "scheduled_start_date": "2024-05-01T10:00:00+00:00",
  "scheduled_end_date": "2024-05-01T10:15:00+00:00",
  "previous_scheduled_start_date": null,
  "previous_scheduled_end_date": null,
  "start_date": "2024-05-01T10:02:00+00:00",
  "end_date": "2024-05-01T10:14:00+00:00",
  "created_at": "2024-04-30T09:00:00+00:00",
  "speciality": {
    "id": 1,
    "name": "General"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Unique consultation identifier |
| `type` | `string` | `appointment` or `chat` |
| `channel` | `string` | `video`, `phone`, `chat` |
| `service_type` | `string\|null` | `scheduled`, `immediate`, etc. `null` for chat consultations |
| `consultation_url` | `string\|null` | Short URL for the video room. `null` for chat consultations |
| `scheduled_start_date` | `ISO8601\|null` | Planned start. `null` for chat consultations |
| `scheduled_end_date` | `ISO8601\|null` | Planned end. `null` for chat consultations |
| `previous_scheduled_start_date` | `ISO8601\|null` | Previous planned start, only populated on `patient_consultation_rescheduled` |
| `previous_scheduled_end_date` | `ISO8601\|null` | Previous planned end, only populated on `patient_consultation_rescheduled` |
| `start_date` | `ISO8601\|null` | Actual start |
| `end_date` | `ISO8601\|null` | Actual end |
| `created_at` | `ISO8601` | Creation timestamp |
| `speciality` | `object` | See [Speciality](speciality.md) |
