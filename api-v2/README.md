# Mediquo API v2

Base URL: `https://sdk-v2.mediquo.com`

## Authentication

All endpoints (except [Authenticate Organization](authenticate/authenticate-organization.md)) require the following header:

```
Authorization: Bearer <access_token>
```

Obtain the token from the [Authenticate Organization](authenticate/authenticate-organization.md) endpoint.

## Endpoints

| Endpoint | Method | URL | Description |
|----------|--------|-----|-------------|
| [Create Appointment](appointments/create-appointment.md) | `POST` | `/appointments` | Create an appointment between a professional and a patient |
| [Create Emergency Phone Call Appointment](appointments/create-emergency-phone-call-appointment.md) | `POST` | `/emergency-calls` | Create an emergency phone call appointment for a patient |
| [Authenticate Organization](authenticate/authenticate-organization.md) | `POST` | `/authenticate` | Obtain a Bearer access token for an organization |
| [Upload Document](documents/upload-document.md) | `POST` | `/documents` | Upload a file to storage and obtain its path for use in other endpoints |
| [Create Immediate Video Call](immediate-videocalls/create-immediate-videocall.md) | `POST` | `/immediate-videocalls` | Create an on-demand video call for a patient |
| [Get Immediate Video Call Schedule Availability](get-immediate-videocall-schedule-availability.md) | `GET` | `/immediate-videocalls/schedules` | List the organization's immediate video call schedules with real-time availability |
| [Authenticate Patient](patients/authenticate-patient.md) | `POST` | `/patients/authenticate` | Obtain a JWT Bearer token for a patient |
| [Create Bulk Patients](patients/create-bulk-patients.md) | `PUT` | `/patients/bulk` | Create up to 100 patients asynchronously |
