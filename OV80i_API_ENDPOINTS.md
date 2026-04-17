# OV80i API Endpoints Documentation (2026-04-17)

1. [Healthcheck & System Status](#1-healthcheck--system-status)
2. [Recipe Management](#2-recipe-management)
3. [Camera & Captures](#3-camera--captures)
4. [Device & System](#4-device--system)
5. [System, Files & Integrations](#5-system-files--integrations)


## 1. Healthcheck & System Status

### GET `/edge/v2/healthcheck`

Returns device health status from the BlueprintAPI process.

**Response** `200`:
```json
{
    "healthcheck": 200
}
```

### GET `/edge/healthcheck`

Legacy healthcheck served by the main RestAPI process.

**Response** `200`:
```json
{
    "healthcheck": 200
}
```

## 2. Recipe Management

### GET `/edge/recipe`
- Get all recipes.

**Response** `200`: Array of recipe objects.
```json
[
  {
    "active": true,
    "blockInstances": [
      {
        "blockType": "segmentation",
        "id": 69
      },
      {
        "blockType": "alignment",
        "id": 35
      },
      {
        "blockType": "classification",
        "id": 36
      }
    ],
    "createdAt": "Mon, 02 Feb 2026 17:15:15 GMT",
    "description": "",
    "editedAt": "Mon, 02 Feb 2026 17:15:19 GMT",
    "hmiType": "default",
    "id": 22,
    "imagingSettings": {
      "acqMode": 1,
      "alignerTriggerDebounceMs": 300,
      "awbBlue": 1024,
      "awbGreen": 1024,
      "awbOp": false,
      "awbRed": 1024,
      "baslerLightSourcePreset": "Daylight5000K",
      "brightness": 100,
      "estimatedCaptureTime": 0,
      "exposure": 50,
      "focalLength": 16,
      "focus": 800,
      "focusDistance": 25,
      "gain": 1,
      "gamma": 50,
      "hdrModeEnabled": false,
      "id": 22,
      "intervalTriggerPeriod": 1000,
      "ledGain": 5,
      "ledMode": 3,
      "ledStrobeMode": false,
      "lensCorrect": false,
      "photometricEnabled": false,
      "photometricMode": 0,
      "previewImagePath": null,
      "rotationMode": 0,
      "smartRoi": null,
      "status": "new",
      "triggerDebounce": 10000,
      "triggerDelay": 0
    },
    "name": "Test Recipe 2",
    "plcRecipeId": 2
  },
  {
    "active": null,
    "blockInstances": [
      {
        "blockType": "alignment",
        "id": 33
      }
    ],
    "createdAt": "Sat, 24 Jan 2026 22:02:24 GMT",
    "description": "",
    "editedAt": "Sun, 25 Jan 2026 17:12:03 GMT",
    "hmiType": "default",
    "id": 21,
    "imagingSettings": {
      "acqMode": 1,
      "alignerTriggerDebounceMs": 300,
      "awbBlue": 1024,
      "awbGreen": 1024,
      "awbOp": false,
      "awbRed": 1024,
      "baslerLightSourcePreset": "Daylight5000K",
      "brightness": 100,
      "estimatedCaptureTime": 0,
      "exposure": 50,
      "focalLength": 16,
      "focus": 800,
      "focusDistance": 25,
      "gain": 1,
      "gamma": 50,
      "hdrModeEnabled": false,
      "id": 21,
      "intervalTriggerPeriod": 1000,
      "ledGain": 5,
      "ledMode": 3,
      "ledStrobeMode": false,
      "lensCorrect": false,
      "photometricEnabled": false,
      "photometricMode": 0,
      "previewImagePath": null,
      "rotationMode": 0,
      "smartRoi": null,
      "status": "new",
      "triggerDebounce": 10000,
      "triggerDelay": 0
    },
    "name": "Test Recipe 3",
    "plcRecipeId": 1
  },
  ...
]
```

---

### POST `/edge/recipe`
- Create a new recipe.

**Request Body**:
```json
{
  "name": "New Recipe",
  "description": "Optional description",
  "plc_recipe_id": 100,
  "active": false,
  "hmi_type": "default",
  "edited_at": "2025-01-01T00:00:00Z"
}
```

**Response** `201`: The created recipe object.
```json
{
  "name": "New Recipe",
  "description": "Optional description",
  "plc_recipe_id": 100,
  "active": false,
  "hmi_type": "default",
  "edited_at": "2025-01-01T00:00:00Z"
}
```
---

### GET `/edge/recipe/<id>`

Get a recipe by database ID.

**Response** `200`: Recipe object.
```json
{
  "active": true,
  "blockInstances": [
    {
      "blockType": "alignment",
      "createdAt": "Tue, 06 Jan 2026 21:19:32 GMT",
      "id": 6,
      "mask": null,
      "name": "Template Image and Alignment",
      "recipeId": 3,
      "updatedAt": null,
      "uuid": "599ed59b-b2fd-4532-a400-833ab19520ee"
    },
    {
      "blockType": "classification",
      "createdAt": "Tue, 06 Jan 2026 21:20:09 GMT",
      "id": 7,
      "mask": null,
      "name": "Model",
      "recipeId": 3,
      "updatedAt": null,
      "uuid": "5f4336c2-407b-449c-8e11-f3805e1cc2c2"
    }
  ],
  "createdAt": "Tue, 06 Jan 2026 21:19:32 GMT",
  "description": "",
  "editedAt": "Mon, 09 Feb 2026 23:10:52 GMT",
  "hmiType": "default",
  "id": 3,
  "imagingSettings": {
    "acqMode": 3,
    "alignerTriggerDebounceMs": 300,
    "awbBlue": 1024,
    "awbGreen": 1024,
    "awbOp": false,
    "awbRed": 1024,
    "baslerLightSourcePreset": "Daylight5000K",
    "brightness": 100,
    "estimatedCaptureTime": 0,
    "exposure": 1000,
    "focalLength": 16,
    "focus": 800,
    "focusDistance": 25,
    "gain": 1,
    "gamma": 50,
    "hdrModeEnabled": false,
    "id": 3,
    "intervalTriggerPeriod": 1000,
    "ledGain": 5,
    "ledMode": 3,
    "ledStrobeMode": false,
    "lensCorrect": false,
    "photometricEnabled": false,
    "photometricMode": 0,
    "previewImagePath": "36bffdfe-4198-4905-a2e3-4e0e382fb6f4.jpg",
    "rotationMode": 0,
    "smartRoi": null,
    "status": "ready",
    "triggerDebounce": 10000,
    "triggerDelay": 0
  },
  "name": "Test",
  "plcRecipeId": 3,
  "remoteStorageSettings": null
}
```

---
### PATCH `/edge/recipe/<id>`
- Update a recipe.

**Request Body** (all fields optional):
```json
{
  "name": "Updated Name",
  "description": "Updated description",
  "plc_recipe_id": 5,
  "active": true,
  "hmi_type": "default"
}
```

**Response** `204`: Empty body.

**Error Responses**:
- `400`: Request body is missing or validation error

---

### DELETE `/edge/recipe/<id>`
- Delete a recipe.

**Response** `204`: Empty body.

---

### GET `/edge/recipe/active`
- Get the currently active recipe.

**Response** `200`: Active recipe object (same schema as `GET /edge/recipe/<id>`).
```json
{
  "active": true,
  "blockInstances": [
    {
      "blockType": "alignment",
      "createdAt": "Mon, 02 Feb 2026 17:15:15 GMT",
      "id": 35,
      "mask": null,
      "name": "Template Image and Alignment",
      "recipeId": 22,
      "updatedAt": null,
      "uuid": "f7783f8a-ef8b-43dc-8531-8d0bdd817d0f"
    },
    {
      "blockType": "classification",
      "createdAt": "Mon, 02 Feb 2026 17:16:59 GMT",
      "id": 36,
      "mask": null,
      "name": "Model",
      "recipeId": 22,
      "updatedAt": null,
      "uuid": "180cb59e-138e-4060-9afc-05559b9cc611"
    },
    {
      "blockType": "segmentation",
      "createdAt": "Wed, 04 Feb 2026 23:50:02 GMT",
      "id": 69,
      "mask": null,
      "name": "Model 2",
      "recipeId": 22,
      "updatedAt": null,
      "uuid": "911949d2-794e-43bf-8bd6-dc32b4e7acd7"
    }
  ],
  "createdAt": "Mon, 02 Feb 2026 17:15:15 GMT",
  "description": "",
  "editedAt": "Mon, 02 Feb 2026 17:15:19 GMT",
  "hmiType": "default",
  "id": 22,
  "imagingSettings": {
    "acqMode": 1,
    "alignerTriggerDebounceMs": 300,
    "awbBlue": 1024,
    "awbGreen": 1024,
    "awbOp": false,
    "awbRed": 1024,
    "baslerLightSourcePreset": "Daylight5000K",
    "brightness": 100,
    "estimatedCaptureTime": 0,
    "exposure": 50,
    "focalLength": 16.0,
    "focus": 800,
    "focusDistance": 25,
    "gain": 1,
    "gamma": 50,
    "hdrModeEnabled": false,
    "id": 22,
    "intervalTriggerPeriod": 1000,
    "ledGain": 5,
    "ledMode": 3,
    "ledStrobeMode": false,
    "lensCorrect": false,
    "photometricEnabled": false,
    "photometricMode": 0,
    "previewImagePath": null,
    "rotationMode": 0,
    "smartRoi": null,
    "status": "new",
    "triggerDebounce": 10000,
    "triggerDelay": 0
  },
  "name": "Ford",
  "plcRecipeId": 2,
  "remoteStorageSettings": null
}
```

---

### PATCH `/edge/recipe/<id>/imaging-settings`
- Update imaging settings for a recipe.

**Request Body** (all fields optional):
```json
{
  "acqMode": 3,
  "alignerTriggerDebounceMs": 300,
  "awbBlue": 1024,
  "awbGreen": 1024,
  "awbOp": false,
  "awbRed": 1024,
  "baslerLightSourcePreset": "Daylight5000K",
  "brightness": 100,
  "estimatedCaptureTime": 0,
  "exposure": 1000,
  "focalLength": 16.0,
  "focus": 800,
  "focusDistance": 25,
  "gain": 1,
  "gamma": 50,
  "hdrModeEnabled": false,
  "intervalTriggerPeriod": 1000,
  "ledGain": 5,
  "ledMode": 3,
  "ledStrobeMode": false,
  "lensCorrect": false,
  "photometricEnabled": false,
  "photometricMode": 0,
  "rotationMode": 0,
  "smartRoi": null,
  "triggerDebounce": 10000,
  "triggerDelay": 0
}
```

**Response** `204`: Empty body.

**Error Responses**:
- `400`: Request body is missing or validation error

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
  "max_images": 5000,
  "include_library_captures": false
}
```

**Response** `200`: Streaming ZIP file (`application/zip`).
- Header `X-Approx-Size-Bytes`: Approximate ZIP size in bytes.

**Error Responses**:
- `400`: Missing payload
- `500`: Export error

---

### POST `/edge/recipe/check-upload-space`
- Check if there is sufficient disk space for a recipe upload before uploading.

**Request Body**:
```json
{
  "file_size_bytes": 104857600
}
```

**Response** `200`:
```json
{
  "success": true,
  "details": "Sufficient disk space available",
  "result": {
    "space_available_bytes": 16000000000,
    "space_required_bytes": 104857600
  }
}
```

**Error Responses**:
- `400`: Missing or invalid `file_size_bytes`
- `507`: Insufficient storage
```json
{
  "success": false,
  "details": "INSUFFICIENT_DISK_SPACE",
  "result": {
    "space_available_bytes": 50000000,
    "space_required_bytes": 104857600
  }
}
```

---

### POST `/edge/recipe/import`
- Import a recipe from a ZIP file. Protected by a lock (one import at a time).

**Request Body**: Multipart form data with `file` field containing the ZIP.

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
- `400`: No file provided or empty file
- `422`: Empty filename or invalid file content
- `429`: Another import is currently in progress
- `500`: Import error

---

### POST `/edge/recipe/change_plc_recipe_id`
- Change the PLC recipe ID for a recipe.

**Request Body**:
```json
{
  "recipe_id": 22,
  "target_plc_recipe_id": 200,
  "restart_pipeline": true
}
```

**Response** `200`: `"Success"`

**Error Responses**:
- `400`: Missing `recipe_id` or `target_plc_recipe_id`, or ID out of range (must be 1–65535)
- `409`: A recipe with that PLC ID already exists

---

### GET `/edge/recipe/deployment-status`
- Get deployment status for each block type in the active recipe.

**Response** `200`:
```json
{
  "alignerDeployed": true,
  "anyClassifierDeployed": true,
  "anySegmenterDeployed": false,
  "classifierDeployed": true,
  "hasClassificationBlocks": true,
  "hasSegmentationBlocks": false,
  "overallDeployed": true,
  "segmenterDeployed": true
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
  "path": "library_captures/2a5e4b8c-2b2d-4e3b-8b3c-0f1c2a3b4c5d.jpg",
  "height": 2048,
  "width": 2592,
  "size": 312345,
  "triggered_by": "manual",
  "source_recipe_id": 22,
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
  "path": "8e4bb7d9-46e1-4e49-9f2b-1e2e3d4c5b6a.jpg",
  "height": 2048,
  "width": 2592,
  "size": 412345,
  "triggered_by": "manual",
  "source_recipe_id": 22,
  "captured_at": "2026-02-10T19:04:12.345678Z",
  "capture_notes": {
    "id": 55,
    "capture_id": 124,
    "notes": "original_filename.jpg",
    "created_at": "2026-02-10T19:04:12.345678Z"
  }
}
```

**Error Responses**:
- `400`: Missing `file`
- `415`: Unsupported file type (only `jpg`, `jpeg`, `png`)
- `422`: Empty filename

---

### GET `/edge/capture_result`
- Get capture results for the active recipe (most recent first).

**Response** `200`: Array of capture result objects.
```json
[
  {
    "id": 501,
    "capture": 123,
    "created_at": "2026-02-10T19:03:13.456789Z",
    "user_metadata": {
      "batch_id": "A-42"
    }
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

### DELETE `/edge/v2/capture`
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
  "successful": 2,
  "failed": 1
}
```

**Error Responses**:
- `400`: Request body is required or validation error
- `500`: Internal server error

---

### GET `/edge/v2/capture/<id>`
- Get a capture by database ID.

**Response** `200`: Capture object.
```json
{
    "cameraSettings": {
        "gain": 1,
        "focus": 800,
        "gamma": 50,
        "awb_op": false,
        "status": "new",
        "awb_red": 1024,
        "acq_mode": 1,
        "awb_blue": 1024,
        "exposure": 50,
        "led_gain": 5,
        "led_mode": 3,
        "awb_green": 1024,
        "recipe_id": 22,
        "smart_roi": null,
        "brightness": 100,
        "focal_length": 16.0,
        "lens_correct": false,
        "rotation_mode": 0,
        "trigger_delay": 0,
        "focus_distance": 25,
        "led_strobe_mode": false,
        "hdr_mode_enabled": false,
        "photometric_mode": 0,
        "trigger_debounce": 10000,
        "preview_image_path": null,
        "photometric_enabled": false,
        "estimated_capture_time": 0,
        "interval_trigger_period": 1000,
        "basler_light_source_preset": "Daylight5000K",
        "aligner_trigger_debounce_ms": 300
    },
    "capturedAt": "2026-02-10T11:48:02.791408Z",
    "height": 1024,
    "ledMode": null,
    "path": "da50ec87-5173-44ee-8b93-82940cf7aa86.jpg",
    "size": 3734137,
    "triggeredBy": "manual",
    "width": 1024,
    "sourceRecipeId": 22,
    "triggerId": null,
    "processEnd": null,
    "acqModeAtCapture": 1,
    "id": 313275,
    "uuid": "50f86272-20a2-4f1c-8bb9-3b222ff296a9",
    "captureNotes": {
        "captureId": 313275,
        "notes": "gear.jpeg",
        "id": 1,
        "createdAt": "2026-02-10T11:48:02.791408Z"
    }
}
```

**Error Responses**:
- `400`: Invalid request
- `404`: Capture not found

---

### POST or PUT `/edge/v2/capture/<capture_id>/notes`
- Create or update notes for a capture.

**Request Body**:
```json
{
  "notes": "Example note text"
}
```

**Response** `201`:
```json
{
  "id": 55,
  "captureId": 123,
  "notes": "Example note text",
  "createdAt": "2026-02-10T19:04:12.345678Z"
}
```

**Error Responses**:
- `400`: Request body is required, missing `notes`, or validation error

---

### GET `/edge/v2/capture/library`
- Get captures for the library view with filtering and pagination.

**Query Parameters** (optional):
- `sort_order` (ascending|descending)
- `captured_from` (ISO datetime)
- `captured_until` (ISO datetime)
- `recipe` (int)
- `id` (int or comma-separated list)
- `inspection_region` (JSON string)
- `search_areas` (comma-separated list)
- `inspection_type` (JSON string)
- `exclude_trainset` (bool)
- `trainset_only` (bool)
- `alignment_fails_only` (bool)
- `capture_notes` (string)
- `pass_or_fail` (bool)
- `trigger_id` (int or comma-separated list)
- `metadata_keys` (comma-separated list)
- `metadata` (JSON string)
- `has_capture_result` (bool)
- `page_start_index` (int)
- `page_size` (int)

**Example Query**:
`<ip_address>/edge/v2/capture/library?sort_order=descending&recipe=22`

**Response** `200`:
```json
{
  "captures": [
        {
            "id": 313276,
            "uuid": "52036d48-8b8d-4bc5-b2d7-623a653e9368",
            "capturedAt": "2026-02-10T12:26:44.083701Z",
            "height": 2160,
            "ledMode": null,
            "path": "bee26eb9-3079-4fc2-a353-c7658fe2c2ee.jpg",
            "size": 6593216,
            "triggeredBy": "manual",
            "width": 3840,
            "sourceRecipeId": 22,
            "triggerId": null,
            "processEnd": null,
            "acqModeAtCapture": 1,
            "recipe": {
                "id": 22,
                "name": "Ford",
                "blockInstances": [
                    {
                        "id": 35,
                        "uuid": "f7783f8a-ef8b-43dc-8531-8d0bdd817d0f",
                        "blockType": "alignment",
                        "classificationHyperparameters": null,
                        "segmentationHyperparameters": null
                    },
                    {
                        "id": 36,
                        "uuid": "180cb59e-138e-4060-9afc-05559b9cc611",
                        "blockType": "classification",
                        "classificationHyperparameters": null,
                        "segmentationHyperparameters": null
                    },
                    {
                        "id": 69,
                        "uuid": "911949d2-794e-43bf-8bd6-dc32b4e7acd7",
                        "blockType": "segmentation",
                        "classificationHyperparameters": null,
                        "segmentationHyperparameters": null
                    }
                ]
            },
            "captureResult": null,
            "isForTraining": null
        },
        ...
  ],
  "totalCapturesCount": 120
}
```

**Error Responses**:
- `400`: Invalid request data
- `500`: Error getting library captures

---

### GET `/edge/v2/capture/library/<capture_id>`
- Get a library capture by ID with associated results and metadata.

**Response** `200`:
```json
{
    "cameraSettings": null,
    "capturedAt": "2026-02-10T11:48:02.791408Z",
    "height": 1024,
    "ledMode": null,
    "path": "da50ec87-5173-44ee-8b93-82940cf7aa86.jpg",
    "size": 3734137,
    "triggeredBy": "manual",
    "width": 1024,
    "sourceRecipeId": 22,
    "triggerId": null,
    "processEnd": null,
    "acqModeAtCapture": 1,
    "id": 313275,
    "uuid": "50f86272-20a2-4f1c-8bb9-3b222ff296a9",
    "captureNotes": {
        "captureId": 313275,
        "notes": "gear.jpeg",
        "id": 1,
        "createdAt": "2026-02-10T11:48:02.791408Z"
    },
    "recipe": {
        "id": 22,
        "name": "Ford",
        "blockInstances": [
            {
                "id": 35,
                "uuid": "f7783f8a-ef8b-43dc-8531-8d0bdd817d0f",
                "blockType": "alignment",
                "classificationHyperparameters": null,
                "segmentationHyperparameters": null
            },
            {
                "id": 36,
                "uuid": "180cb59e-138e-4060-9afc-05559b9cc611",
                "blockType": "classification",
                "classificationHyperparameters": null,
                "segmentationHyperparameters": null
            },
            {
                "id": 69,
                "uuid": "911949d2-794e-43bf-8bd6-dc32b4e7acd7",
                "blockType": "segmentation",
                "classificationHyperparameters": null,
                "segmentationHyperparameters": null
            }
        ]
    },
    "captureResult": null,
    "alignmentResults": [],
    "classificationResults": [],
    "segmentationResults": [],
    "ocrResults": [],
    "isForTraining": null
}
```

**Error Responses**:
- `400`: Invalid request data
- `500`: Error getting library capture

---

### GET `/edge/v2/capture/library/filters/recipe/<recipe_id>`
- Get available filter options for library captures by recipe.

**Response** `200`:
```json
{
    "id": 22,
    "searchAreas": [
        {
            "blockId": 35,
            "alignmentParamsId": 18,
            "name": "Search Area 1",
            "main": true,
            "searchAreaBbox": [
                192,
                108,
                3648,
                2052
            ],
            "id": 18
        }
    ],
    "inspectionRegions": [
        {
            "name": "ROI",
            "inspectionTypeId": 26,
            "angle": 0.0,
            "bbox": [
                100.0,
                100.62784582725897,
                200.0,
                200.0
            ],
            "fabricjsObject": {
                "rx": 0,
                "ry": 0,
                "top": 100.63,
                "fill": "transparent",
                "left": 100,
                "type": "rect",
                "angle": 0,
                "flipX": false,
                "flipY": false,
                "skewX": 0,
                "skewY": 0,
                "width": 200,
                "height": 200,
                "scaleX": 1,
                "scaleY": 1,
                "shadow": null,
                "stroke": "yellow",
                "opacity": 1,
                "originX": "left",
                "originY": "top",
                "version": "5.5.2",
                "visible": true,
                "fillRule": "nonzero",
                "paintFirst": "fill",
                "strokeWidth": 7.5,
                "strokeLineCap": "butt",
                "strokeUniform": true,
                "strokeLineJoin": "miter",
                "backgroundColor": "",
                "strokeDashArray": null,
                "strokeDashOffset": 0,
                "strokeMiterLimit": 4,
                "globalCompositeOperation": "source-over"
            },
            "locked": false,
            "id": 58,
            "inspectionType": {
                "name": "Inspection Type 1",
                "blockId": 36,
                "id": 26
            }
        },
        {
            "name": "ROI",
            "inspectionTypeId": 59,
            "angle": 0.0,
            "bbox": [
                100.0,
                100.0,
                200.0,
                200.0
            ],
            "fabricjsObject": {
                "rx": 0,
                "ry": 0,
                "top": 100,
                "fill": "transparent",
                "left": 100,
                "type": "rect",
                "angle": 0,
                "flipX": false,
                "flipY": false,
                "skewX": 0,
                "skewY": 0,
                "width": 200,
                "height": 200,
                "scaleX": 1,
                "scaleY": 1,
                "shadow": null,
                "stroke": "yellow",
                "opacity": 1,
                "originX": "left",
                "originY": "top",
                "version": "5.5.2",
                "visible": true,
                "fillRule": "nonzero",
                "paintFirst": "fill",
                "strokeWidth": 7.5,
                "strokeLineCap": "butt",
                "strokeUniform": true,
                "strokeLineJoin": "miter",
                "backgroundColor": "",
                "strokeDashArray": null,
                "strokeDashOffset": 0,
                "strokeMiterLimit": 4,
                "globalCompositeOperation": "source-over"
            },
            "locked": false,
            "id": 91,
            "inspectionType": {
                "name": "Inspection Type 1",
                "blockId": 69,
                "id": 59
            }
        }
    ],
    "inspectionTypes": [
        {
            "name": "Inspection Type 1",
            "blockId": 36,
            "id": 26,
            "blockClassificationClasses": [
                {
                    "createdAt": "2026-02-02T17:16:59.198133Z",
                    "color": "#00DD00",
                    "labelName": "Pass",
                    "inspectionTypeId": 26,
                    "id": 44,
                    "inspectionType": {
                        "name": "Inspection Type 1",
                        "blockId": 36,
                        "id": 26
                    }
                },
                {
                    "createdAt": "2026-02-02T17:16:59.198133Z",
                    "color": "#EE0000",
                    "labelName": "Fail",
                    "inspectionTypeId": 26,
                    "id": 45,
                    "inspectionType": {
                        "name": "Inspection Type 1",
                        "blockId": 36,
                        "id": 26
                    }
                }
            ],
            "blockSegmentationClasses": []
        },
        {
            "name": "Inspection Type 1",
            "blockId": 69,
            "id": 59,
            "blockClassificationClasses": [],
            "blockSegmentationClasses": [
                {
                    "color": "#EE0000",
                    "labelName": "Fail",
                    "inspectionTypeId": 59,
                    "maskPixelValue": 1,
                    "id": 10,
                    "inspectionType": {
                        "name": "Inspection Type 1",
                        "blockId": 69,
                        "id": 59
                    }
                }
            ]
        }
    ]
}
```

**Error Responses**:
- `400`: recipe_id must be a positive integer
- `404`: Recipe not found
- `500`: Error getting filter options

---

### DELETE `/edge/v2/capture/all`
- Delete captures from the database and disk (destructive). Without query parameters deletes all captures; alignment template images are always excluded; training-set captures are excluded unless `includeTrainset=true`. Progress updates are published via MQTT on `capture/delete/progress`.

**Query Parameters** (all optional):
- `recipe` (int): Filter by recipe ID
- `captured_from` / `captured_until` (ISO datetime): Time window
- `pass_or_fail` (bool)
- `metadata` (JSON string): key/value filter on user metadata
- `metadata_keys` (comma-separated list): keys that must be present
- `search_areas` (comma-separated ints)
- `inspection_type` (JSON string)
- `inspection_region` (JSON string)
- `alignment_fails_only` (bool)
- `includeTrainset` (bool, default `false`)

**Response** `200`:
```json
{
  "successful": 120,
  "failed": 0
}
```

**Error Responses**:
- `500`: Internal server error

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
  "name": "c111"
}
```

---

### POST `/edge/device/name`
- Set the device name.

**Request Body**:
```json
{
  "name": "c111"
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

### GET `/edge/v2/device/network`
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

### POST `/edge/v2/device/network`
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

---

### GET `/edge/v2/device/storage`
- Get storage usage and SD endurance (if available).

**Response** `200`:
```json
{
  "total": 187576299520,
  "used": 165543305216,
  "free": 16101564416,
  "percent": 91.1,
  "endurance": null
}
```

---

### GET `/edge/v2/device/serial_number`
- Get the device serial number.

**Response** `200`:
```json
{
  "serial_number": "gsac059110"
}
```

---

### GET `/edge/v2/device/activation`
- Get device activation status.

**Response** `200`:
```json
{
  "activated": true
}
```

---

### POST `/edge/v2/device/activation`
- Mark the device as activated.

**Request Body**: None.

**Response** `200`:
```json
{
  "activated": true
}
```

---

### GET `/edge/v2/device/hostname`
- Get the host hostname.

**Response** `200`:
```json
{
  "hostname": "ov80i-gsac059110"
}
```

---

### POST `/edge/v2/device/hostname`
- Set the host hostname.

**Request Body**:
```json
{
  "hostname": "ov80i-gsac059110"
}
```

**Response** `200`:
```json
{
  "hostname": "ov80i-gsac059110"
}
```

**Error Responses**:
- `400`: Missing or invalid hostname
- `500`: Failed to set hostname

---

### GET `/edge/v2/device/camera_type`
- Get the camera model type.

**Response** `200`:
```json
{
  "camera_type": "ov80i"
}
```

---

### GET `/edge/v2/device/time`
- Get system time in microseconds.

**Response** `200`:
```json
{
  "now_us": 1770728704381184
}
```

---

### POST `/edge/v2/device/time`
- Set system time in microseconds.

**Request Body**:
```json
{
  "now_us": 1770728704381184
}
```

**Response** `200`: Resulting system time in microseconds.
```json
{
  "now_us": 1770728704381184
}
```

**Error Responses**:
- `400`: Missing payload

---

### GET `/edge/v2/device/network/link`
- Get network link diagnostics (driver, speed, duplex, etc.) from ethtool.

**Response** `200`: Link-info object with vendor-specific fields.

**Error Responses**:
- `500`: Failed to read link info

---

### GET `/edge/v2/device/ntp`
- Get NTP configuration.

**Response** `200`:
```json
{
    "enabled": true,
    "servers": [
        "a.st1.ntp.br"
    ]
}
```

---

### POST `/edge/v2/device/ntp`
- Update NTP configuration.

**Request Body**:
```json
{
    "enabled": true,
    "servers": [
        "a.st1.ntp.br"
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

### POST `/edge/v2/device/restart`
- Restart the device.

**Response** `200`: Empty JSON object.
```json
{}
```

---

### GET `/edge/v2/device/version`
- Get the software version from `APP_VERSION`.

**Response** `200`:
```json
{
  "version": "ov80i_2025.12"
}
```

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

### POST `/edge/create_manual_log_entry`
- Create a manual log entry for debugging.

**Request Body**:
```json
{
  "msg": "Manual log entry from API"
}
```

**Response** `200`:
```
Success
```

**Error Responses**:
- `400`: Missing or invalid JSON payload
- `500`: Failed to create log entry

---

## 5. System, Files & Integrations

### GET `/edge/v2/files/<path:file_path>`
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

**Response** `200`:
```json
[
    {
        "filename": "rootfs_update_runner.ovupdate",
        "last_modified": 1765498113.0,
        "name": "rootfs_update_runner-v1",
        "size": 5632,
        "valid": true
    },
    {
        "filename": "OV80i_v6_20260102-192035_full_jetpack_upgrade.ovupdate",
        "last_modified": 1767388913.0,
        "name": "rootfs-image-v6",
        "size": 11813861376,
        "valid": true
    },
    {
        "filename": "autocam-update.ovupdate",
        "last_modified": 1743618601.715508,
        "name": "autocam-v2025.3.2-OV80i",
        "size": 9842132480,
        "valid": true
    }
]
```

---

### DELETE `/edge/update`
- Delete an update file.

**Request Body**:
```json
{
  "filename": "autocam-update.ovupdate"
}
```

**Response** `200`: Empty body.

**Error Responses**:
- `400`: Invalid filename or is a directory
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

**Error Responses**:
- `200`: When `status` is `error`, `logs` is included in the response

---

### POST `/edge/update/start`
- Start a system update from an already-uploaded update file.

**Request Body**:
```json
{
  "filename": "autocam-update.ovupdate"
}
```

**Response** `200`: Empty body (update kicked off).

**Error Responses**:
- `400`: Missing or invalid JSON payload
- `422`: One of: `file_not_found`, `outdated_update`, or `insufficient_disk_space`
  ```json
  {
    "error": "insufficient_disk_space",
    "message": "Insufficient disk space available. 2.1GB available, 5GB required for update."
  }
  ```
- `500`: Unexpected error

---

### GET `/edge/update/preferences`
- Get update preferences.

**Response** `200`:
```json
{
  "auto_install_updates": false
}
```

---

### POST `/edge/update/preferences`
- Update preferences.

**Request Body**:
```json
{
  "auto_install_updates": true
}
```

**Response** `200`:
```json
{
  "auto_install_updates": true
}
```

**Error Responses**:
- `400`: Missing or invalid JSON payload

---

### POST `/uploader/update`
- Stream-upload an update file to the dedicated uploader server (port 5002, proxied at `/uploader/*`).

**Request Body**: Multipart form data with the update file. The endpoint relies on request streaming — do not use clients that buffer the full file in memory.

**Response** `200`: Empty body.

**Error Responses**:
- `422`: Invalid filename or content length

---

### GET `/edge/certificates`
- List HTTPS certificates.

**Response** `200`:
```json
[
  {
    "id": 1,
    "name": "OV80i Default",
    "createdAt": "2026-02-10T19:10:00Z",
    "commonName": "OV80i",
    "ipAddress": "192.168.15.111",
    "issuerName": "OV80i",
    "issuerOrganization": "Overview",
    "organization": "Overview",
    "organizationalUnit": "Engineering",
    "country": "US",
    "validFrom": "2026-02-10T19:10:00Z",
    "validTo": "2027-02-10T19:10:00Z",
    "isActive": true
  }
]
```

**Error Responses**:
- `500`: Failed to list certificates

---

### POST `/edge/certificates/upload`
- Upload a PKCS#12 bundle, or a certificate+key pair.

**Request Body**: Multipart form data with:
- `name` (string, required)
- `password` (string, optional) — needed for encrypted bundles/keys
- Either a `pkcs12` file, or both `certificate` and `key` files

**Response** `201`:
```json
{
  "message": "Certificate uploaded successfully",
  "certificate": { "id": 2, "name": "...", "...": "..." }
}
```

**Error Responses**:
- `400`: Missing name/files or invalid certificate format
- `500`: Failed to upload certificate

---

### POST `/edge/certificates/self-signed`
- Create a self-signed certificate.

**Request Body**:
```json
{
  "name": "Demo",
  "common_name": "OV80i",
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
  "certificate": { "id": 3, "name": "Demo", "...": "..." }
}
```

**Error Responses**:
- `400`: Missing body or validation error
- `500`: Failed to create self-signed certificate

---

### DELETE `/edge/certificates/<certificate_id>`
- Delete a certificate.

**Response** `204`: Empty body.

**Error Responses**:
- `400`: Active certificate cannot be deleted while HTTPS is enabled
- `404`: Certificate not found
- `500`: Failed to delete certificate

---

### GET `/edge/certificates/<certificate_id>/download`
- Download a certificate file (`application/x-x509-ca-cert`).

**Response** `200`: Certificate file bytes.

**Error Responses**:
- `404`: Certificate not found
- `500`: Failed to download certificate

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
  "active_protocol": "profinet"
}
```

**Error Responses**:
- `500`: Failed to get protocol

---

### POST `/edge/industrial_ethernet/protocol`
- Set the active industrial ethernet protocol.

**Request Body**:
```json
{
  "active_protocol": "profinet"
}
```

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
  "profinet_device_name": "OV80i"
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
  "profinet_device_name": "OV80i"
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
- Create or update the Node-RED flow.

**Request Body**:
```json
{
  "nodered_flow": {
    "flow": [],
    "basic_mode": true,
    "basic_flow_id": "",
    "basic_flow_metadata": {}
  },
  "classification_data": null,
  "segmentation_data": null,
  "ocr_data": null,
  "refresh_nodered": false
}
```

**Response** `200`:
```json
{
  "message": "Flow created/updated successfully"
}
```

**Error Responses**:
- `400`: Invalid payload
- `500`: Failed to create/update flow

---

### POST `/edge/nodered/flow/toggle-basic-mode`
- Toggle basic mode for the active recipe flow.

**Request Body**:
```json
{
  "disabled": true
}
```

**Response** `200`:
```json
{
  "noderedFlow": {
    "basicMode": false,
    "flow": []
  }
}
```

**Error Responses**:
- `400`: Invalid payload
- `500`: Failed to convert flow

---

### POST `/edge/remote_storage/upload_test`
- Queue a remote storage upload test.

**Request Body**:
```json
{
  "protocol": "sftp",
  "host": "127.0.0.1",
  "port": 22,
  "username": "user",
  "password": "pass",
  "remote_dir": "/",
  "enabled": false,
  "add_trigger_id": false
}
```

**Response** `200`:
```json
{
  "message": "Upload test queued successfully"
}
```

**Error Responses**:
- `400`: Request body is required
- `500`: Upload test failed

---

### POST `/edge/remote_storage/settings`
- Upsert remote storage settings for the active recipe.

**Request Body**:
```json
{
  "protocol": "sftp",
  "host": "127.0.0.1",
  "port": 22,
  "username": "user",
  "password": "pass",
  "remote_dir": "/",
  "enabled": false,
  "add_trigger_id": false
}
```

**Response** `200`:
```json
{
  "message": "Remote storage settings upserted successfully",
  "id": 1,
  "recipe_id": 22
}
```

**Error Responses**:
- `400`: Request body is required or validation error
- `500`: Failed to upsert remote storage settings

---

### POST `/edge/external-gpu/check-connectivity`
- Probe connectivity to an external GPU at a given IP.

**Request Body**:
```json
{
  "ip_address": "192.168.1.50"
}
```

**Response** `200`:
```json
{
  "success": true,
  "message": "Connected"
}
```

**Error Responses**:
- `400`: Missing or malformed `ip_address`
- `500`: Internal server error

---

### POST `/edge/external-gpu/pre-training-check`
- Run a pre-training connectivity check against the configured external GPU. Skipped if external GPU is not enabled.

**Request Body**: None.

**Response** `200`:
```json
{
  "success": true,
  "message": "Connected"
}
```

**Error Responses**:
- `500`: Internal server error

---

### GET `/edge/v2/logs`
- Download system logs as a streamed ZIP (docker logs, container metadata, dmesg, system info, browser diagnostics).

**Query Parameters** (all optional):
- `last_n_seconds` (int, default `86400`)
- `user_agent` (string)
- `location` (string)
- `language` (string)
- `client_timestamp` (int, ms)

**Response** `200`: Streamed ZIP (`application/zip`).

---

## Common HTTP Status Codes

| Status | Meaning | Typical Use in This API |
| --- | --- | --- |
| 200 | OK | Successful GET/POST with response body |
| 201 | Created | Resource created (e.g., uploads, create) |
| 204 | No Content | Successful request with empty body |
| 400 | Bad Request | Missing or invalid payload/params |
| 401 | Unauthorized | Missing or invalid auth token |
| 403 | Forbidden | Authenticated but not allowed |
| 404 | Not Found | Resource does not exist |
| 409 | Conflict | Duplicate or state conflict |
| 422 | Unprocessable Entity | Validation failed or invalid file |
| 429 | Too Many Requests | Busy lock or throttling |
| 500 | Internal Server Error | Unhandled server error |
| 504 | Gateway Timeout | Long-running operation timed out |

---

## Server Architecture and Ports

| Service | Container/Process | Port | Notes |
| --- | --- | --- | --- |
| Nginx | proxy | 80 | Front door reverse proxy |
| Edge API | edge | 5001 | Main RestAPI (`/edge/*`) |
| Blueprint API | blueprint-api | 5004 | Heavy routes (`/edge/v2/*`) |
| Upload Server | uploader | 5002 | Streaming updates (`/uploader/*`) |
| Node-RED | node-red | 1880 | Flow runtime (`/node-red/*`) |
| Node-RED Control | node-red-control | 3000 | Admin/control endpoints |
| PostgREST | postgrest | 9000 | Database REST API (`/postgrest/*`) |
| MQTT WebSocket | mqtt | 9001 | WebSocket broker (`/mqtt`) |
| Prometheus Export | prometheus-export | 9101 | Metrics (`/prometheus-export/*`) |
| Logs | logs | 8081 | Dozzle logs (`/logs`) |
| H264 NAL | h264-nal | 9003 | Video stream (`/h264-nal`) |
| Frontend | frontend | 8000 | UI (`/`) |
