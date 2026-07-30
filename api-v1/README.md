# Mediquo API v1

Base URL: `https://sdk.mediquo.com/v1`

## Authentication

Organization endpoints require these headers:

```http
x-api-key: <your-api-key>
x-secret-key: <your-secret-key>
```

The credentials must belong to an organization with API v1 enabled. Missing or
invalid credentials return `401 Unauthorized`.

## Endpoints

| Endpoint | Method | URL | Description |
|----------|--------|-----|-------------|
| [List Organization Webhooks](organizations/list-organization-webhooks.md) | `GET` | `/organizations/webhooks/` | List the authenticated organization's active webhook subscriptions |
| [Create Organization Webhook](organizations/create-organization-webhook.md) | `POST` | `/organizations/webhooks/` | Create a webhook subscription for the authenticated organization |
| [Delete Organization Webhook](organizations/delete-organization-webhook.md) | `DELETE` | `/organizations/webhooks/{webhookId}` | Soft-delete an organization webhook subscription |
| [Update Organization Webhook](organizations/update-organization-webhook.md) | `PUT` | `/organizations/webhooks/{webhookId}` | Replace an organization webhook subscription's configuration |
