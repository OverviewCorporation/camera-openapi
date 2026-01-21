# Camera OpenAPI

## Usage

1. Copy and paste the [spec](./openapi.yml) onto any openapi editors (e.g. [Swagger Editor](https://editor.swagger.io)) to visualize or interact with the interface.
2. Change `servers[].url` to the camera host at your needs.

## Scenarios

1. Trigger capture via HTTP

    Please follow the endpoint `POST /edge/camera/capture`.

2. Fetch segmentation mask details

    Please follow the endpoint `GET /edge/v2/block/segmentation/results`.