# OV20i API Endpoints Documentation (2026-02-10)

1. [Healthcheck & System Status](#1-healthcheck--system-status)
2. [Recipe / Pipeline Management](#2-recipe--pipeline-management)
3. [Camera & Captures](#3-camera--captures)
4. [Device & System](#4-device--system)
5. [System, Files & Integrations](#5-system-files--integrations)


## 1. Healthcheck & System Status

### GET `/edge/healthcheck`

Returns system health status, data generation progress, and training progress.

**Response** `200`:
```json
{
    "status": "ok",
    "generate_data_progress": null,
    "training_progress": null
}
```

---

### GET `/edge/version`
- Get the software version from `APP_VERSION`.

**Response** `200`:
```json
{
  "version": "ov20i_2025.12"
}
```

---

### GET `/edge/swap_memory_stats`
- Get swap memory statistics.

**Response** `200`:
```json
{
  "total": 2048.0,
  "used": 512.0,
  "free": 1536.0,
  "percent_used": 25.0,
  "unit": "MB"
}
```

---

## 2. Recipe / Pipeline Management

### POST `/edge/pipeline`
- Create a new pipeline/recipe.

**Request Body**:
```json
{
  "name": "New Recipe",
  "description": "Recipe description",
  "recipe_type": "standard"
}
```

**Response** `201`:
```json
{
  "success": true,
  "details": "",
  "result": {}
}
```

**Error Responses**:
- `400`: Invalid or missing JSON body

---

### POST `/edge/pipeline/activate`
- Activate a recipe by database ID. Legacy endpoint (same handler as `/edge/recipe/activate`).

**Request Body**:
```json
{
  "id": 22
}
```

**Response** `200`:
```json
{
  "success": true,
  "details": ""
}
```

**Error Responses**:
- `400`: Invalid or missing payload
- `408`: Recipe switch timed out
- `429`: Another recipe switch is in progress
- `500`: Recipe switch failed

---

### POST `/edge/recipe/activate`
- Activate a recipe by its database ID. Protected by a lock to prevent concurrent switches.

**Request Body**:
```json
{
  "id": 22
}
```

**Response** `200`:
```json
{
  "success": true,
  "details": ""
}
```

**Error Responses**:
- `400`: Invalid or missing payload
- `408`: Recipe switch timed out
- `429`: Another recipe switch is in progress
- `500`: Recipe switch failed

---

### POST `/edge/api/recipes/<recipe_id>/activate`
- Activate a recipe by its user-facing PLC recipe ID (not the database ID).

**Path Parameters**:
- `recipe_id` (int): The PLC recipe ID

**Request Body**: None.

**Response** `200`:
```json
{
  "success": true,
  "recipe_id": 2
}
```

**Error Responses**:
- `404`: Recipe not found
- `429`: Another recipe switch is in progress
- `500`: Error activating recipe
- `504`: Timeout while activating recipe

---

### POST `/edge/recipe/export`
- Export a recipe as a streaming ZIP download.

**Request Body**:
```json
{
  "recipe_id": 22,
  "max_images": 100,
  "include_library_captures": false
}
```

**Response** `200`: Streaming ZIP file (`application/zip`).
- Header `X-Approx-Size-Bytes`: Approximate ZIP size in bytes.

**Error Responses**:
- `400`: Missing payload
- `500`: Export error

---

### POST `/edge/recipe/import`
- Import a recipe from a ZIP or tarball file. Protected by a lock (one import at a time).

**Request Body**: Multipart form data with `file` field containing the ZIP/tarball.

**Response** `200`:
```json
{
  "success": true,
  "details": "",
  "result": 5
}
```
Where `result` is the new recipe database ID.

**Error Responses**:
- `400`: No file provided
- `422`: Empty filename or invalid recipe content
- `429`: Another import is currently in progress
- `500`: Import error

---

### POST `/edge/recipe/change_plc_recipe_id`
- Change the PLC recipe ID for a recipe.

**Request Body**:
```json
{
  "recipe_id": 22,
  "target_plc_recipe_id": 5,
  "restart_pipeline": true
}
```

**Response** `200`: `"Success"`

**Error Responses**:
- `400`: Missing `recipe_id` or `target_plc_recipe_id`, or ID out of range (must be 0–65535)
- `409`: A recipe with that PLC ID already exists

---

### POST `/edge/recipe/rename`
- Rename a recipe.

**Request Body**:
```json
{
  "recipe_id": 22,
  "name": "Updated Recipe Name"
}
```

**Response** `200`: `"Success"`

**Error Responses**:
- `400`: Missing parameters or empty/whitespace name
- `404`: Recipe not found

---

### GET `/edge/recipe/deployable`
- Check whether the active recipe is deployable.

**Response** `200`:
```json
{
  "deployable": true
}
```

---

## 3. Camera & Captures

### POST `/edge/camera/capture`
- Capture a single frame from the camera and save it as a new capture.

**Request Body** (optional):
```json
{
  "subdir": "library_captures"
}
```

**Response** `200`: Capture object.
```json
{
  "id": 123,
  "file_path": "captures/capture_123.jpg",
  "recipe_id": 22,
  "captured_at": "2026-02-10T19:03:12.345678Z"
}
```

---

### POST `/edge/captures/upload`
- Upload an image file and create a new capture.

**Request Body**: Multipart form data with `file` field.

**Response** `200`: Capture object with notes containing the original filename.
```json
{
  "id": 124,
  "file_path": "captures/capture_124.jpg",
  "recipe_id": 22,
  "capture_notes": {
    "notes": "original_filename.jpg"
  }
}
```

**Error Responses**:
- `400`: Missing `file`
- `415`: Unsupported file type (only `jpg`, `jpeg`, `png`)
- `422`: Empty filename

---

### POST `/edge/captures/delete`
- Delete multiple captures by ID.

**Request Body**:
```json
{
  "ids": [123, 124, 125]
}
```

**Response** `200`:
```json
{
  "successfull": [123, 124],
  "failed": [125]
}
```

**Error Responses**:
- `400`: Request body is required or validation error

---

### GET `/edge/capture_result`
- Get capture results for the active recipe (most recent first).

**Response** `200`: Array of capture result objects.
```json
[
  {
    "id": 1,
    "capture_id": 123,
    "recipe_id": 22,
    "created_at": "2026-02-10T19:03:12.345678Z"
  }
]
```

---

### GET `/edge/capture_result/user_metadata/keys`
- Get a list of user metadata keys present in capture results (sorted by frequency).

**Response** `200`:
```json
[
  "batch_id",
  "operator"
]
```

---

### POST `/edge/camera/do`
- Set a digital output (DIO) pin value.

**Request Body**:
```json
{
  "pin": 1,
  "value": 1
}
```

**Response** `200`: Empty body.

**Error Responses**:
- `400`: Missing `pin` or `value`, or hardware error

---

### GET `/edge/camera/led_color`
- Get the current LED color value.

**Response** `200`:
```
3
```

**Valid Values**:
- `0`: Off
- `1`: Green
- `2`: Orange
- `3`: Yellow (green + orange)

---

### POST `/edge/camera/led_color`
- Set the LED color value.

**Request Body**:
```json
{
  "value": 3
}
```

**Valid Values**:
- `0`: Off
- `1`: Green
- `2`: Orange
- `3`: Yellow (green + orange)

**Response** `200`: Empty body.

**Error Responses**:
- `400`: Missing or invalid payload

---

## 4. Device & System

### GET `/edge/device/name`
- Get the device name.

**Response** `200`:
```json
{
  "name": "camera_01"
}
```

---

### POST `/edge/device/name`
- Set the device name.

**Request Body**:
```json
{
  "name": "camera_01"
}
```

**Response** `200`:
```json
{
    "success": true
}
```

**Error Responses**:
- `400`: Missing or invalid JSON payload

---

### GET `/edge/device/network`
- Get current network configuration.

**Response** `200`:
```json
{
  "active": {
    "address": "192.168.15.111",
    "dns": [],
    "gateway": "192.168.15.1",
    "mode": "static",
    "netmask": "255.255.255.0"
  },
  "configuration": {
    "address": "192.168.15.111",
    "dns": [],
    "gateway": "192.168.15.1",
    "mode": "static",
    "netmask": "255.255.255.0"
  },
  "mac_address": "3C:6D:66:01:82:0C"
}
```

---

### POST `/edge/device/network`
- Update network configuration.

**Request Body**:
```json
{
  "address": "192.168.15.111",
  "dns": [],
  "gateway": "192.168.15.1",
  "mode": "static",
  "netmask": "255.255.255.0"
}
```

**Response** `200`: Empty body.

**Error Responses**:
- `400`: Invalid payload
- `500`: Validation error (e.g., missing required fields)

---

### GET `/edge/device/storage`
- Get storage usage and SD endurance (if available).

**Response** `200`:
```json
{
  "total": 187576299520,
  "used": 45000000000,
  "free": 142576299520,
  "percent": 24.0,
  "endurance": null
}
```

---

### GET `/edge/device/serial_number`
- Get the device serial number.

**Response** `200`:
```json
{
  "serial_number": "gsac059110"
}
```

---

### GET `/edge/device/activation`
- Get device activation status.

**Response** `200`:
```json
{
  "activated": true
}
```

---

### GET `/edge/device/hostname`
- Get the host hostname.

**Response** `200`:
```json
{
  "hostname": "ov20i-gsac059110"
}
```

---

### POST `/edge/device/hostname`
- Set the host hostname.

**Request Body**:
```json
{
  "hostname": "ov20i-gsac059110"
}
```

**Response** `200`:
```json
{
  "hostname": "ov20i-gsac059110"
}
```

**Error Responses**:
- `400`: Missing or invalid hostname
- `500`: Failed to set hostname

---

### GET `/edge/device/camera_type`
- Get the camera model type.

**Response** `200`:
```json
{
  "camera_type": "ov20i"
}
```

---

### GET `/edge/device/time`
- Get system time in microseconds.

**Response** `200`:
```json
{
  "now_us": 1770728704381184
}
```

---

### POST `/edge/device/time`
- Set system time in microseconds.

**Request Body**:
```json
{
  "now_us": 1770728704381184
}
```

**Response** `200`:
```json
{
  "now_us": 1770728704381184
}
```

**Error Responses**:
- `400`: Missing payload

---

### GET `/edge/device/ntp`
- Get NTP configuration.

**Response** `200`:
```json
{
    "enabled": true,
    "servers": [
        "0.pool.ntp.org",
        "1.pool.ntp.org"
    ]
}
```

---

### POST `/edge/device/ntp`
- Update NTP configuration.

**Request Body**:
```json
{
    "enabled": true,
    "servers": [
        "0.pool.ntp.org",
        "1.pool.ntp.org"
    ]
}
```

**Response** `200`:
```
Successfully set NTP configuration
```

**Error Responses**:
- `400`: Missing payload

---

### POST `/edge/device/restart`
- Restart the device.

**Response** `200`: Empty JSON object.
```json
{}
```

---

## 5. System, Files & Integrations

### GET `/edge/files/<path:file_path>`
- Download a file from the data directory.

**Query Parameters** (optional):
- `cache_max_age` (int): Cache duration in seconds
- `width` (int): Resize width for JPEG images

**Response** `200`: File bytes.

**Error Responses**:
- `404`: File not found

---

### GET `/edge/update`
- List update files in the update directory.

**Response** `200`: JSON array of update file objects.

---

### DELETE `/edge/update`
- Delete an update file.

**Request Body**:
```json
{
  "filename": "update_v2.tar.gz"
}
```

**Response** `200`: Empty string.

**Error Responses**:
- `400`: Missing JSON body, is a directory, or invalid filename
- `404`: File not found

---

### GET `/edge/update/status`
- Get update status and logs when an update fails.

**Response** `200`:
```json
{
  "status": "finished"
}
```

When `status` is `error`, `logs` is also included in the response.

---

### GET `/edge/certificates`
- List HTTPS certificates.

**Response** `200`: JSON array of certificate objects with `is_active` flag.
```json
[
  {
    "id": 1,
    "name": "My Certificate",
    "common_name": "ov20i.local",
    "is_active": true,
    "expires_at": "2027-02-10T00:00:00Z"
  }
]
```

**Error Responses**:
- `500`: Failed to list certificates

---

### POST `/edge/certificates/upload`
- Upload a PKCS#12 file or certificate + key pair.

**Request Body**: Multipart form data with:
- `name` (required): Certificate name
- `password` (optional): Password for PKCS#12 files
- Either `pkcs12` file, OR both `certificate` and `key` files

**Response** `201`:
```json
{
  "message": "Certificate uploaded successfully",
  "certificate": {}
}
```

**Error Responses**:
- `400`: Missing name, no file, invalid format, no cert/key files
- `500`: Upload failure

---

### POST `/edge/certificates/self-signed`
- Create a self-signed certificate.

**Request Body**:
```json
{
  "name": "Self-Signed Cert",
  "common_name": "ov20i.local",
  "organization": "Overview",
  "organizationalUnit": "Engineering",
  "country": "US",
  "validity_days": 365,
  "ip_address": "192.168.15.111"
}
```

**Response** `200`:
```json
{
  "success": true,
  "certificate": {}
}
```

**Error Responses**:
- `400`: Missing body or validation error
- `500`: Creation failure

---

### DELETE `/edge/certificates/<certificate_id>`
- Delete a certificate.

**Response** `204`: Empty body.

**Error Responses**:
- `400`: Active certificate cannot be deleted while HTTPS is enabled
- `404`: Certificate not found
- `500`: Failed to delete certificate

---

### POST `/edge/certificates/<certificate_id>/set_active`
- Set a certificate as the active HTTPS certificate.

**Response** `200`:
```json
{
  "message": "Certificate successfully set as active",
  "certificate_id": 1
}
```

**Error Responses**:
- `404`: Certificate not found
- `500`: Failed to set certificate as active

---

### GET `/edge/certificates/<certificate_id>/download`
- Download a certificate file by ID.

**Response** `200`: Certificate file download (`application/x-x509-ca-cert`).

**Error Responses**:
- `404`: Certificate not found, or certificate file not found
- `500`: Failed to download certificate

---

### GET `/edge/web_server/protocols`
- Get HTTP/HTTPS enablement status.

**Response** `200`:
```json
{
  "http": true,
  "https": false
}
```

---

### POST `/edge/web_server/protocols`
- Update HTTP/HTTPS enablement.

**Request Body**:
```json
{
  "http": true,
  "https": true
}
```

**Response** `200`:
```json
{
  "success": true
}
```

**Error Responses**:
- `400`: Missing or invalid fields, or both disabled
- `500`: Failed to update protocol

---

### GET `/edge/industrial_ethernet/protocol`
- Get the active industrial ethernet protocol.

**Response** `200`:
```json
{
  "active_protocol": "ethernetip"
}
```

**Valid Values** for `active_protocol`:
- `"ethernetip"`: EtherNet/IP
- `"profinet"`: PROFINET

**Error Responses**:
- `500`: Failed to get protocol

---

### POST `/edge/industrial_ethernet/protocol`
- Set the active industrial ethernet protocol.

**Request Body**:
```json
{
  "active_protocol": "ethernetip"
}
```

**Valid Values** for `active_protocol`:
- `"ethernetip"`: EtherNet/IP
- `"profinet"`: PROFINET

**Response** `200`: `"Success"`

**Error Responses**:
- `400`: Missing or invalid payload
- `500`: Failed to set protocol

---

### GET `/edge/download/industrial_ethernet/ethernet_ip_eds`
- Download the Ethernet/IP EDS ZIP.

**Response** `200`: ZIP file (`application/zip`).

**Error Responses**:
- `404`: EDS file not found
- `500`: Download failed

---

### GET `/edge/download/industrial_ethernet/profinet_gsdml`
- Download the Profinet GSDML ZIP.

**Response** `200`: ZIP file (`application/zip`).

**Error Responses**:
- `404`: GSDML file not found
- `500`: Download failed

---

### GET `/edge/industrial_ethernet/profinet/device_name`
- Get the Profinet device name.

**Response** `200`:
```json
{
  "profinet_device_name": "OV20i"
}
```

**Error Responses**:
- `500`: Failed to get Profinet device name

---

### POST `/edge/industrial_ethernet/profinet/device_name`
- Set the Profinet device name (only when Profinet is active).

**Request Body**:
```json
{
  "profinet_device_name": "OV20i"
}
```

**Response** `200`: `"Success"`

**Error Responses**:
- `400`: Profinet protocol not active or invalid payload
- `500`: Failed to set Profinet device name

---

### GET `/edge/nodered/flow`
- Get the Node-RED flow for the active recipe.

**Response** `200`:
```json
{
  "nodered_flow": {
    "flow": [...],
    "basic_mode": true,
    "basic_flow_id": "0b0c1b6f-1fca-4803-b689-6e2ba356268b",
    "basic_flow_metadata": {...}
  }
}
```

**Error Responses**:
- `404`: Flow not found

---

### POST `/edge/nodered/flow`
- Create or update the Node-RED flow for the active recipe.

**Request Body**:
```json
{
  "nodered_flow": {
    "flow": {},
    "basic_mode": false,
    "basic_flow_id": "",
    "basic_flow_metadata": {}
  }
}
```

**Response** `200`: Empty string.

**Error Responses**:
- `400`: Invalid payload

---

### PATCH `/edge/nodered/flow`
- Partially update the Node-RED flow for the active recipe.

**Request Body**: Same as POST `/edge/nodered/flow`.

**Response** `200`: Empty string.

**Error Responses**:
- `400`: Invalid payload

---

### POST `/edge/nodered/update_flow`
- Callback from Node-RED when a user updates a flow. Updates DB and syncs.

**Request Body**: Flow JSON payload.

**Response** `200`: Empty string.

**Error Responses**:
- `400`: Invalid or missing JSON body

---

### GET `/edge/remote_storage/upload_test`
- Test remote storage upload by creating a temporary file and queuing it.

**Response** `200`: `"OK"`

**Error Responses**:
- `500`: Queue is full or failed to queue file for upload

---

### GET `/edge/environmental_variables`
- Get environment variables from the Node-RED env file.

**Response** `200`:
```
LINE_CODE=line1
CAMERA_TIMEZONE=GMT-3
DATE_INSTALLED=2026-01-01
```

**Error Responses**:
- `500`: Failed to read environment variables

---

### POST `/edge/environmental_variables`
- Overwrite environment variables in the Node-RED env file.

**Request Body**:
```
LINE_CODE=line2
CAMERA_TIMEZONE=GMT-5
DATE_INSTALLED=2026-01-01
```

**Response** `200`:
```
Environment file updated successfully
```

**Error Responses**:
- `400`: Empty body
- `500`: Failed to write environment variables

---

## Common HTTP Status Codes

| Status | Meaning | Typical Use in This API |
| --- | --- | --- |
| 200 | OK | Successful GET/POST with response body |
| 201 | Created | Resource created (e.g., uploads, create) |
| 204 | No Content | Successful request with empty body |
| 400 | Bad Request | Missing or invalid payload/params |
| 401 | Unauthorized | Missing or invalid auth token |
| 403 | Forbidden | Authenticated but not allowed or invalid file extension |
| 404 | Not Found | Resource does not exist |
| 405 | Method Not Allowed | HTTP method not supported for this endpoint |
| 409 | Conflict | Duplicate or state conflict |
| 415 | Unsupported Media Type | Invalid file type |
| 422 | Unprocessable Entity | Validation failed or invalid file |
| 429 | Too Many Requests | Busy lock or throttling |
| 500 | Internal Server Error | Unhandled server error |
| 504 | Gateway Timeout | Long-running operation timed out |

---

## Server Architecture and Ports

| Service | Container/Process | Port | Notes |
| --- | --- | --- | --- |
| Nginx | proxy | 80 | Front door reverse proxy |
| Edge API | edge | 5001 | Main RestAPI (`/edge/*`) — all endpoints |
| Upload Server | uploader | 5002 | Streaming updates (`/uploader/*`) |
| Node-RED | node-red | 1880 | Flow runtime (`/node-red/*`) |
| Node-RED Control | node-red-control | 3000 | Admin/control endpoints |
| PostgREST | postgrest | 9000 | Database REST API (`/postgrest/*`) |
| MQTT WebSocket | mqtt | 9001 | WebSocket broker (`/mqtt`) |
| Prometheus Export | prometheus-export | 9101 | Metrics (`/prometheus-export/*`) |
| Logs | logs | 8081 | Dozzle logs (`/logs`) |
| H264 NAL | h264-nal | 9003 | Video stream (`/h264-nal`) |
| Frontend | frontend | 8000 | UI (`/`) |

---

**Note**: Unlike the OV80i, the OV20i branch does **not** have a separate Blueprint API service. All API endpoints are served by the main Edge API on port 5001. There is no `/edge/v2/*` routing split.
