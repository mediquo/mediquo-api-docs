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
| [Get Appointment Consultants](appointments/get-consultants.md) | `GET` | `/appointments/consultants` | List the professionals available to receive appointments |
| [Get Schedule Availability](appointments/get-schedule-availability.md) | `GET` | `/appointments/schedule-availability` | List the organization's bookable appointment slots |
| [Cancel Appointment](appointments/cancel-appointment.md) | `DELETE` | `/appointments/{code}` | Cancel an appointment by the organization's appointment code |
| [Authenticate Organization](authenticate/authenticate-organization.md) | `POST` | `/authenticate` | Obtain a Bearer access token for an organization |
| [Upload Document](documents/upload-document.md) | `POST` | `/documents` | Upload a file to storage and obtain its path for use in other endpoints |
| [Create Immediate Video Call](immediate-videocalls/create-immediate-videocall.md) | `POST` | `/immediate-videocalls` | Create an on-demand video call for a patient |
| [Get Immediate Video Call Schedule Availability](get-immediate-videocall-schedule-availability.md) | `GET` | `/immediate-videocalls/schedules` | List the organization's immediate video call schedules with real-time availability |
| [Authenticate Patient](patients/authenticate-patient.md) | `POST` | `/patients/authenticate` | Obtain a JWT Bearer token for a patient |
| [Create Bulk Patients](patients/create-bulk-patients.md) | `PUT` | `/patients/bulk` | Create up to 100 patients asynchronously |
| [List Patient Appointments](patients/list-patient-appointments.md) | `GET` | `/patients/{code}/appointments` | List a patient's appointments, filterable and paginated |
