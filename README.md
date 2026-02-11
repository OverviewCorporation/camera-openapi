# Camera OpenAPI

OpenAPI specifications for ov80i camera APIs.

## Specs

| File | Description |
|------|-------------|
| [ov80i_openapi.yml](./ov80i_openapi.yml) | Known OV80i API — recipes, captures, device config, integrations |

## Usage

1. Copy and paste a spec onto any OpenAPI editor (e.g. [Swagger Editor](https://editor.swagger.io)) to visualize or interact with the interface.
2. Change `servers[].url` to the camera host at your needs.

## Use Cases

1. Trigger capture via HTTP

    Please follow the endpoint `POST /edge/camera/capture`.

2. Fetch segmentation mask details

    Please follow the endpoint `GET /edge/v2/block/segmentation/results`.
