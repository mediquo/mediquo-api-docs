# Webhooks

All webhook events share the same top-level envelope:

```json
{
  "type": "<event_type>",
  "occurred_on": 1715000000,
  "payload": { ... }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `type` | `string` | Event identifier (see each payload doc) |
| `occurred_on` | `integer` | Unix timestamp of when the event occurred |
| `payload` | `object` | Event-specific data |

## Available events

| Event type | Payload doc | Trigger |
|------------|-------------|---------|
| `patient_consultation_created` | [PatientConsultation](patient-consultation.md) | Appointment booked |
| `patient_consultation_cancelled` | [PatientConsultation](patient-consultation.md) | Appointment cancelled |
| `patient_consultation_rescheduled` | [PatientConsultation](patient-consultation.md) | Appointment rescheduled |
| `patient_consultation_finished` | [PatientConsultation](patient-consultation.md) | Appointment or chat consultation finished |
| `patient_consultation_no_show` | [PatientConsultationNoShowed](patient-consultation-no-showed.md) | Patient did not attend |
| `patient_message_received` | [PatientPendingMessage](patient-pending-message.md) | Unread patient message in room |
| `patient_immediate_video_call_assigned` | [PatientImmediateCallAssigned](patient-immediate-call-assigned.md) | Immediate video call assigned to professional |
| `professional_block_created` | [ProfessionalBlock](professional-block.md) | Professional availability block created |
| `professional_block_updated` | [ProfessionalBlock](professional-block.md) | Professional availability block updated |
| `professional_block_deleted` | [ProfessionalBlock](professional-block.md) | Professional availability block deleted |

## Shared objects

Reusable objects referenced across multiple payloads:

- [Consultation object](objects/consultation.md)
- [Patient object](objects/patient.md)
- [Professional object](objects/professional.md)
- [Report object](objects/report.md)
- [Prescription object](objects/prescription.md)
- [Speciality object](objects/speciality.md)
