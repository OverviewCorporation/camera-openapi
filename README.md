# Camera OpenAPI

OpenAPI specifications and API reference docs for Overview camera APIs.

## API Reference Docs

Human-readable endpoint references for each camera model. These cover the full API surface and are not tied to a specific firmware version.

| File | Camera | Description |
|------|--------|-------------|
| [OV20i_API_ENDPOINTS.md](./OV20i_API_ENDPOINTS.md) | OV20i | All `/edge/*` endpoints served by the Edge API |
| [OV80i_API_ENDPOINTS.md](./OV80i_API_ENDPOINTS.md) | OV80i | All `/edge/v2/*` endpoints served by the Blueprint API |

## OpenAPI Specs

Each spec file is named after the firmware release tag it documents (the
same tag you'll find on a camera via `GET /edge/version` for OV20i or
`GET /edge/v2/device/version` for OV80i). Pick the file that matches your
device's firmware; opening the matching file directly is the way to see the
API at that version, rather than diffing git history on a single file.

### OV20i

| File | Release tag |
|------|-------------|
| [ov20i_v2026.5.5_openapi.yml](./ov20i_v2026.5.5_openapi.yml) | `v2026.5.5` |
| [ov20i_v2026.5.4_openapi.yml](./ov20i_v2026.5.4_openapi.yml) | `v2026.5.4` |
| [ov20i_v2026.5.3_openapi.yml](./ov20i_v2026.5.3_openapi.yml) | `v2026.5.3` |
| [ov20i_v2026.5.2_openapi.yml](./ov20i_v2026.5.2_openapi.yml) | `v2026.5.2` |
| [ov20i_v2026.5.1_openapi.yml](./ov20i_v2026.5.1_openapi.yml) | `v2026.5.1` |
| [ov20i_v2026.5.0_openapi.yml](./ov20i_v2026.5.0_openapi.yml) | `v2026.5.0` |
| [ov20i_v2026.3.0_openapi.yml](./ov20i_v2026.3.0_openapi.yml) | `v2026.3.0` |
| [ov20i_v2026.2.1_openapi.yml](./ov20i_v2026.2.1_openapi.yml) | `v2026.2.1` |
| [ov20i_v2026.2.0_openapi.yml](./ov20i_v2026.2.0_openapi.yml) | `v2026.2.0` |
| [ov20i_v2026.1.0_openapi.yml](./ov20i_v2026.1.0_openapi.yml) | `v2026.1.0` |

### OV80i

| File | Release tag |
|------|-------------|
| [ov80i_v2026.6.0_openapi.yml](./ov80i_v2026.6.0_openapi.yml) | `v2026.6.0-OV80i` |
| [ov80i_v2026.4.4_openapi.yml](./ov80i_v2026.4.4_openapi.yml) | `v2026.4.4-OV80i` |
| [ov80i_v2026.4.3_openapi.yml](./ov80i_v2026.4.3_openapi.yml) | `v2026.4.3-OV80i` |
| [ov80i_v2026.4.2_openapi.yml](./ov80i_v2026.4.2_openapi.yml) | `v2026.4.2-OV80i` |
| [ov80i_v2026.4.1_openapi.yml](./ov80i_v2026.4.1_openapi.yml) | `v2026.4.1-OV80i` |
| [ov80i_v2026.4.0_openapi.yml](./ov80i_v2026.4.0_openapi.yml) | `v2026.4.0-OV80i` |
| [ov80i_v2026.3.12_openapi.yml](./ov80i_v2026.3.12_openapi.yml) | `v2026.3.12-OV80i` |
| [ov80i_v2026.3.4_openapi.yml](./ov80i_v2026.3.4_openapi.yml) | `v2026.3.4-OV80i` |
| [ov80i_v2026.3.3_openapi.yml](./ov80i_v2026.3.3_openapi.yml) | `v2026.3.3-OV80i` |
| [ov80i_v2026.3.2_openapi.yml](./ov80i_v2026.3.2_openapi.yml) | `v2026.3.2-OV80i` |
| [ov80i_v2026.3.1_openapi.yml](./ov80i_v2026.3.1_openapi.yml) | `v2026.3.1-OV80i` |
| [ov80i_v2026.3.0_openapi.yml](./ov80i_v2026.3.0_openapi.yml) | `v2026.3.0-OV80i` |

## Usage

1. Copy and paste a spec onto any OpenAPI editor (e.g. [Swagger Editor](https://editor.swagger.io)) to visualize or interact with the interface.
2. Change `servers[].url` to the camera host as needed.

## Known Issues

- **`requestBody` semantics warning**: Both OV20i and OV80i specs trigger the validation warning *"requestBody does not have well-defined semantics for GET, HEAD and DELETE operations"*. This is expected — the cameras' actual APIs use request bodies in DELETE requests, so the specs intentionally reflect this behavior despite it being unconventional per the OpenAPI specification.
