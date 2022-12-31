# Boilerplate - OpenAPI

A small project demonstrating using a simple OpenApi specification that has been divided into smaller focused files with
supporting operations to verify the correctness of the spec, compose the spec into a distributable form with web-ui, and
generate code from the composed specification.

In this project, 2 docker images are used. The first is declared in this project and is used to validate and bundle the
spec before building the HTML file ready to host. The second docker image is the official [openapi-generator-cli](https://github.com/OpenAPITools/openapi-generator) as provided by [OpenAPITools](https://github.com/OpenAPITools).

## Spec Operations

Build the image
```bash
docker build . -t open-api-builder
```

Build outputs
```bash
docker run -v ${PWD}/spec:/spec -v ${PWD}/docs:/docs -e SPEC_SERVICE_NAME=demo -e SPEC_FILE_NAME=main.yml open-api-builder
```

## Template and Generator Operations

### Some Contextual Information that Could Help

There are 4 components to the `openapi-generator` process:
1. The cli tool that brokers the process
2. A "generator"
3. A template
4. A Specification

In general, don't mess with generators unless you specifically need to. For context, I've never encountered a need to
modify or create a generator. In areas that have seemed challenging, the difficulty has usually come down to poor
expression of the specification.

Refer to [https://openapi-generator.tech/docs/templating/](https://openapi-generator.tech/docs/templating/) for
additional information about templating.

Any `openapi-generator` cli command can be run in the docker image if you correctly structure the command issued to and
through docker. Most of the time you will just need the following 2 commands:

Output a template (example for `typescript-axios` template)
```bash
docker run --rm \
    -v "${PWD}/templates:/templates" \
    openapitools/openapi-generator-cli \
    author template \
    -g typescript-axios \
    -o /templates/typescript-axios
```
Refer to The OpenAPI [Generators List](https://openapi-generator.tech/docs/generators/) for the valid options for the `-g` argument.

Run generation with a specific template
```bash
docker run --rm \
    -v "${PWD}/docs/dist:/local" \
    -v "${PWD}/clients:/clients" \
    -v "${PWD}/templates:/templates" \
    openapitools/openapi-generator-cli \
    generate \
    -i /local/main.yml \
    -g typescript-axios \
    -t /templates/typescript-axios \
    -o /clients/typescript-axios
```

## Q&A

### Couldn't you just use the npm package [@openapitools/openapi-generator-cli](https://www.npmjs.com/package/@openapitools/openapi-generator-cli)?
Sure. That package, however, requires Java and I just don't think the possible gains are worth the effort. The irony
here is not lost on me.

### Ok, cool, so why use NPM at all?
You can get the exact same results using the official [redocly/cli](https://hub.docker.com/r/redocly/cli) image. I use 
the NPM package in order to batch the commands together. Just a preference.
