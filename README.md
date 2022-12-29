# Boilerplate - OpenAPI


## Operations

Build the image
```bash
docker build . -t open-api-builder
```

Build outputs
```bash
docker run -v ${PWD}/spec:/spec -v ${PWD}/docs:/docs -e SPEC_SERVICE_NAME=demo -e SPEC_FILE_NAME=main.yml open-api-builder
```

Host the page
```bash

```