# Camera OpenAPI

OpenAPI specifications for Overview camera APIs.

## Specs

Each spec file is named after the firmware release tag it documents (the
same tag you'll find on a camera via `GET /edge/v2/device/version`). Pick
the file that matches your device's firmware release; opening the matching
file directly is the way to see the API at that version, rather than
diffing git history on a single file.

### OV20i

| File | Release tag | Released | Notes |
|------|-------------|----------|-------|
| [ov20i_v2026.2.1_openapi.yml](./ov20i_v2026.2.1_openapi.yml) | `v2026.2.1` | 2026-02-23 | Shared release tag; also covers OV20i builds off the same commit |

### OV80i

| File | Release tag | Released | Notes |
|------|-------------|----------|-------|
| [ov80i_v2026.4.0_openapi.yml](./ov80i_v2026.4.0_openapi.yml) | `v2026.4.0-OV80i` | 2026-04-09 | Current; includes `GET /edge/v2/device/network/link` |
| [ov80i_v2026.3.1_openapi.yml](./ov80i_v2026.3.1_openapi.yml) | `v2026.3.1-OV80i` | 2026-03-10 | Last OV80i release before `/edge/v2/device/network/link` was added (2026-03-11) — also a faithful reference for any earlier `-OV80i` firmware since the documented surface didn't change between them |

## Usage

1. Copy and paste a spec onto any OpenAPI editor (e.g. [Swagger Editor](https://editor.swagger.io)) to visualize or interact with the interface.
2. Change `servers[].url` to the camera host at your needs.

## Known Issues

- **`requestBody` semantics warning**: Both OV20i and OV80i specs trigger the validation warning *"requestBody does not have well-defined semantics for GET, HEAD and DELETE operations"*. This is expected — the cameras' actual APIs use request bodies in DELETE requests, so the specs intentionally reflect this behavior despite it being unconventional per the OpenAPI specification.