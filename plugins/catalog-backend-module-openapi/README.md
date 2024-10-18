# Catalog Backend Module to resolve $refs in yaml documents

* == extension module to the Catalog backend / resolve $refs | ".yaml"
  * allows
    * ".yaml" -- can be split into -- multiple files & reference them
      * how does it work?
        * -- via -- UrlReader -> they are bundled
        * -- stored as a -- 1! specification
  * uses
    * OpenAPI specifications
    * AsyncAPI specifications

## Installation

### Install the package

```bash
# From your Backstage root directory
yarn --cwd packages/backend add @backstage/plugin-catalog-backend-module-openapi
```

### Adding the plugin to your `packages/backend`

```ts title="packages/backend/src/index.ts"
backend.add(import('@backstage/plugin-catalog-backend-module-openapi'));
```

* -> `jsonSchemaRefPlaceholderResolver` -- is added for the -- placeholder resolver keys
  * `asyncapi`
    * == `$asyncapi` placeholder is used | referencing your AsyncAPI specification
  * `openapi`
    * == `$openapi` placeholder is used | referencing your OpenAPI specification 
    * _Example:_

    ```yaml
    apiVersion: backstage.io/v1alpha1
    kind: API
    metadata:
      name: example
      description: Example API
    spec:
      type: openapi
      lifecycle: production
      owner: team
      definition:
        $openapi: ./spec/openapi.yaml # by using $openapi Backstage will now resolve all $ref instances
    ```

### Adding the plugin to your `packages/backend` (old backend system)

#### **jsonSchemaRefPlaceholderResolver**

* how to add the placeholder resolver?
  * import `jsonSchemaRefPlaceholderResolver` | `backend` package's `src/plugins/catalog.ts`
  * add 

  ```ts
  builder.setPlaceholderResolver('openapi', jsonSchemaRefPlaceholderResolver);
  builder.setPlaceholderResolver('asyncapi', jsonSchemaRefPlaceholderResolver);
  ```
* allows using
  * `$asyncapi` placeholder is used | referencing your AsyncAPI specification
  * `$openapi` placeholder is used | referencing your OpenAPI specification
    * _Example:_

    ```yaml
    apiVersion: backstage.io/v1alpha1
    kind: API
    metadata:
      name: example
      description: Example API
    spec:
      type: openapi
      lifecycle: production
      owner: team
      definition:
        $openapi: ./spec/openapi.yaml # by using $openapi Backstage will now resolve all $ref instances
    ```

#### **OpenAPIRefProcessor** (deprecated)

* steps
  * import `OpenApiRefProcessor` | `backend` package's `src/plugins/catalog.ts`
  * add

  ```ts
  builder.addProcessor(
    OpenApiRefProcessor.fromConfig(env.config, {
      logger: env.logger,
      reader: env.reader,
    }),
  );
  ```
