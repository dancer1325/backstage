# @backstage/plugin-api-docs-module-protoc-gen-doc

* `ApiDefinitionWidgets`
  * for the following projects:
    * [grpc-docs](https://github.com/gendocu-com/grpc-docs)
  * allows
    * render the generated `protoc-gen-doc` descriptors -- with the -- `grpc-docs` package
      * -> use `type: grpc-docs`

## Setup

```sh
# From your Backstage root directory
yarn --cwd packages/app add @backstage/plugin-api-docs-module-protoc-gen-doc
```

## Add the GrpcDocsApiWidget to your apis

* Add the widget | your `apiDocsConfigRef`

  ```ts
  import { grpcDocsApiWidget } from '@backstage/plugin-api-docs-module-protoc-gen-doc';
  // packages/app/apis.ts
  export const apis: AnyApiFactory[] = [
    createApiFactory({
      api: apiDocsConfigRef,
      deps: {},
      factory: () => {
        // load the default widgets
        const definitionWidgets = defaultDefinitionWidgets();
        // add the grpc-docs api widget to the definition widgets
        definitionWidgets.push(grpcDocsApiWidget);
        return {
          getApiDefinitionWidget: (apiEntity: ApiEntity) => {
            // find the widget for the type of api entity
            return definitionWidgets.find(d => d.type === apiEntity.spec.type);
          },
        };
      },
    }),
  ];
  ```

### Set the type in your api entities


```yaml
apiVersion: backstage.io/v1alpha1
kind: API
metadata:
  name: hello-world
  description: Hello World example for gRPC
spec:
  type: grpc-docs         # to be able to render the descriptor -- with -- `grpc-docs` package
  lifecycle: deprecated
  owner: foo
  definition:
    $text: https://fetch-my-json.com/awesome-resource.json
```
