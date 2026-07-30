# Organization Webhooks

The Organization Webhooks API lets an organization manage its outgoing webhook
subscriptions. A subscription selects an event (`type`), a delivery channel
(`channel`), and channel-specific configuration (`meta`).

The API only manages subscriptions. Event delivery, payloads, and shared webhook
objects are documented in the [Webhooks guide](../../webhooks/README.md).

## End-to-end flow

1. Create an `http` subscription with the URL of your receiving endpoint in
   `meta.url`.
2. When the subscribed event occurs, mediQuo sends a `POST` request to that URL.
3. Your endpoint processes the event and returns a `2xx` response.

## Authentication and scope

All operations use the API v1 organization credentials:

```http
x-api-key: <your-api-key>
x-secret-key: <your-secret-key>
```

The organization is inferred from these credentials, so there is no
`organizationId` in the URL. Organizations can only list and manage their own
subscriptions.

## Webhook resource

```json
{
  "id": "c0766228-bde8-447e-8284-040e79e952a8",
  "type": "report_created",
  "channel": "http",
  "meta": {
    "url": "https://client.example.com/hooks/report"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` (UUID v4) | Server-generated subscription identifier |
| `type` | `string` enum | Event subscription key |
| `channel` | `string` enum | Delivery channel: `http` or `email` |
| `meta` | `object` | Channel-specific configuration |

Multiple subscriptions may use the same `type` and `channel`. Each subscription
is an independent resource with its own `id`.

## Subscription types

The subscription `type` is not always the same as the `type` in the delivered
webhook body.

| Subscription `type` | Delivered payload `type` | Payload |
|---------------------|--------------------------|---------|
| `appointment_cancelled` | `patient_consultation_cancelled` | [Patient consultation](../../webhooks/patient-consultation.md) |
| `appointment_created` | `patient_consultation_created` | [Patient consultation](../../webhooks/patient-consultation.md) |
| `appointment_finished` | `patient_consultation_finished` | [Patient consultation](../../webhooks/patient-consultation.md) |
| `appointment_no_showed` | `patient_consultation_no_show` | [Patient consultation no-show](../../webhooks/patient-consultation-no-showed.md) |
| `appointment_rescheduled` | `patient_consultation_rescheduled` | [Patient consultation](../../webhooks/patient-consultation.md) |
| `chat_consultation_finished` | `patient_consultation_finished` | [Patient consultation](../../webhooks/patient-consultation.md) |
| `immediate_video_call_assigned` | `patient_immediate_video_call_assigned` | [Immediate video call assigned](../../webhooks/patient-immediate-call-assigned.md) |
| `message_unread` | `patient_message_received` | [Patient message received](../../webhooks/patient-message-received.md) |
| `patient_message_received` | `patient_message_received` | [Patient message received](../../webhooks/patient-message-received.md) |
| `prescription_created` | `prescription_created` | [Prescription created](../../webhooks/prescription-created.md) |
| `professional_block_created` | `professional_block_created` | [Professional block](../../webhooks/professional-block.md) |
| `professional_block_deleted` | `professional_block_deleted` | [Professional block](../../webhooks/professional-block.md) |
| `professional_block_updated` | `professional_block_updated` | [Professional block](../../webhooks/professional-block.md) |
| `report_created` | `patient_report_sent` | [Report created](../../webhooks/report-created.md) |
| `triage_position_updated` | `patient_waiting_room_position_update` | [Triage position updated](../../webhooks/triage-position-updated.md) |

`appointment_finished` and `chat_consultation_finished` deliver the same payload
type. Likewise, `message_unread` and `patient_message_received` both deliver
`patient_message_received`. If you subscribe to both values in either pair,
inspect the payload to distinguish their origin.

## Channel metadata

### HTTP

The `http` channel requires `meta.url`. `meta.headers` is optional and adds
custom headers to outgoing requests.

```json
{
  "url": "https://client.example.com/hooks/report",
  "headers": {
    "X-Tenant": "acme"
  }
}
```

### Email

The `email` channel accepts an empty `meta` object. It also accepts the optional
fields `mailer`, `subject`, `title`, `sub_title`, `cta_button_text`, and
`cta_button_url`.

Email delivery is currently implemented for `appointment_created`,
`appointment_cancelled`, `immediate_video_call_assigned`, `message_unread`, and
`triage_position_updated`. Use `http` for other subscription types.

## Operations

- [List organization webhooks](list-organization-webhooks.md)
- [Create an organization webhook](create-organization-webhook.md)
- [Update an organization webhook](update-organization-webhook.md)
- [Delete an organization webhook](delete-organization-webhook.md)

Deleting a subscription performs a soft delete. A deleted subscription is no
longer returned by the list endpoint and no longer receives events.
