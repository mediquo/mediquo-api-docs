# Upload File

Uploads a file to S3 storage and returns the storage path and file name. The returned `file_path` and `file_name` can be used as references in other endpoints (e.g. `preconsultation_documents`).

## Request

**Method:** `POST`
**URL:** `https://sdk-v2.mediquo.com/documents/v2/upload-file`

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer <access_token>` — obtained from the [Authenticate Organization](../authenticate/authenticate-organization.md) endpoint |
| `x-api-key` | Yes | Organization API key |
| `x-secret-key` | Yes | Organization secret key |
| `Content-Type` | Yes | `multipart/form-data` |

### Body

Sent as `multipart/form-data`.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `file` | `file` | Yes | File to upload. Maximum size: 300 MB. Allowed types: documents (`pdf`, `doc`, `docx`, `xls`, `xlsx`, `ppt`, `pptx`, `odt`, `ods`, `odp`, `txt`, `rtf`), images (`jpg`, `jpeg`, `png`, `bmp`, `webp`, `tiff`, `tif`, `heic`, `gif`), medical images (`dcm`, `dicom`, `nii`, `mhd`, `mha`, `hdr`, `img`, `nrrd`, `vtk`, `svs`, `ndpi`), video (`mp4`, `mov`, `m4v`, `avi`, `wmv`, `mpeg`, `mpg`, `webm`), audio (`mp3`, `m4a`, `aac`, `wav`, `ogg`, `amr`), compressed (`zip`, `rar`, `7z`, `tar`, `gz`, `bz2`) |

## Responses

### `200 OK`

```json
{
  "url": "appointment-documents/a1b2c3d4-e5f6-7890-abcd-ef1234567890/analysis.pdf",
  "data": {
    "file_path": "appointment-documents/a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "file_name": "analysis.pdf"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `url` | `string` | Full storage path of the uploaded file (`file_path/file_name`) |
| `data.file_path` | `string` | Storage folder path — use this as `file_path` in `preconsultation_documents` |
| `data.file_name` | `string` | Original file name — use this as `file_name` in `preconsultation_documents` |

### Errors

| Status | Condition |
|--------|-----------|
| `401 Unauthorized` | Missing or invalid `Authorization` Bearer token, `x-api-key`, or `x-secret-key` |
| `413 Request Entity Too Large` | File exceeds the maximum allowed size (300 MB) |
| `415 Unsupported Media Type` | File type is not in the allowed list |
| `422 Unprocessable Entity` | `file` field is missing from the request |

## Example

```bash
curl -X POST "https://sdk-v2.mediquo.com/documents/v2/upload-file" \
  -H "Authorization: Bearer <access_token>" \
  -H "x-api-key: <your-api-key>" \
  -H "x-secret-key: <your-secret-key>" \
  -F "file=@/path/to/analysis.pdf"
```
