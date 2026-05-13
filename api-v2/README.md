# Mediquo API v2

Base URL: `https://sdk-v2.mediquo.com`

## Authentication

All endpoints (except `/authenticate`) require a Bearer token obtained from the [Authenticate Organization](authenticate-organization.md) endpoint.

```
Authorization: Bearer <access_token>
```

## Endpoints

| Endpoint | Method | URL | Description |
|----------|--------|-----|-------------|
| [Authenticate Organization](authenticate-organization.md) | `POST` | `/authenticate` | Obtain a Bearer access token for an organization |
