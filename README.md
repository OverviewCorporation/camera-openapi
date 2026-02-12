# Camera OpenAPI

OpenAPI specifications for Overview camera APIs.

## Specs

| File | Description |
|------|-------------|
| [ov20i_2026_02_10_openapi.yml](./ov20i_2026_02_10_openapi.yml) | Known OV20i APIs |
| [ov80i_2026_02_01_openapi.yml](./ov80i_2026_02_01_openapi.yml) | Known OV80i APIs |

## Usage

1. Copy and paste a spec onto any OpenAPI editor (e.g. [Swagger Editor](https://editor.swagger.io)) to visualize or interact with the interface.
2. Change `servers[].url` to the camera host at your needs.

## Known Issues

- **`requestBody` semantics warning**: Both OV20i and OV80i specs trigger the validation warning *"requestBody does not have well-defined semantics for GET, HEAD and DELETE operations"*. This is expected — the cameras' actual APIs use request bodies in DELETE requests, so the specs intentionally reflect this behavior despite it being unconventional per the OpenAPI specification.