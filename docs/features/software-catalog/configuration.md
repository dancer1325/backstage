---
id: configuration
title: Catalog Configuration
description: Documentation on Software Catalog Configuration
---

## Processors

* allows
  * ingesting tasks | catalog
    * _Example:_ reading raw entity data -- from a -- remote source, parsing it, transforming
  it, and validating it
* are configured | `catalog.processors`

### Static Location Configuration

* 👀simplest configuration for the catalog 👀
  * _Example:_ `@backstage/create-app` template, is to declaratively add locations pointing to
  YAML files with [static configuration](../../conf/index.md).
* they are added -- via -- `catalog.locations`
  * types
    * `url`
      * -- is handled by -- `UrlReaderProcessor`
        * := standard processor / included with the catalog
        * -> NO needed processor configuration
      * requirements
        * [integration](../../integrations/index.md) -- to retrieve a -- given URL
          * _Example:_ configure the [GitHub integration](../../integrations/github/locations.md)

            ```yaml
            catalog:
              locations:
                - type: url
                  target: https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/components/artist-lookup-component.yaml
            ```
* can NOT be removed -- through the -- catalog locations API
  * -> you MUST remove them | configuration
* Syntax errors or other types of errors | `catalog-info.yaml`
  * logged for investigation
  * NOT cause aborting
* if MULTIPLE `catalog-info.yaml` files / SAME `metadata.name` -> 
  * 1! will be processed
  * OTHERS will be skipped

### Local File (`type: file`) Configurations

* `locations[*].type=file`
  * allows
    * bringing in content -- from the -- local file system 
  * uses
    * local development
    * test setups
    * example data
      * ⚠️NOT for production data ⚠️
  * `target`
    * does NOT accept
      * placeholders 
        * _Example:_ `$text`
    * reference other files / -- relative to the -- current file
* _Examples:_
  * [catalog example](https://github.com/backstage/backstage/tree/master/packages/catalog-model/examples)
  * 
    ```yaml
    catalog:
      locations:
        - type: file
          target: ../../examples/all.yaml       # `../../`    is normally `packages/backend/` 
    ```

:::note
There might be cases where you need to test some `file` configurations in a Docker container. In a case like this, as the backend is serving the frontend in a default setup, the path would be from the root. Also, you need to **make sure to copy your files into your container**.

Using the example above that would look like this:

```yaml
catalog:
  locations:
    - type: file
      target: ./examples/all.yaml
```

:::

### Integration Processors

* Integrations
  * provide
    * mechanism -- to handle -- `locations[*].type=url` -- for an -- external provider 
  * [integrations documentation](../../integrations/index.md) 
* way to handle external providers
  * -- via --
    * integration processors or
    * integration processors + additional processors (_Example:_ GitHub [discovery](../../integrations/github/discovery.md))

### Custom Processors

* use cases
  * ingest entities -- from an -- existing system / ALREADY tracking software
    * existing system -- is converted to -- Backstage's descriptor format
* [External Integrations documentation](external-integrations.md)

## Catalog Rules

* ONLY  allowed entities / can be ingested
  * by default
    * `Component`
    * `API`
    * `Location`
  * 👀if you want to ingest others -> you need to add rules | catalog👀
    * allowed entities
      * | ANY location
        * `Component`
        * `API`
        * `Location`
        * `Template`
      * | `org-data.yaml`
        * `Group`
        * -- are read as a -- statically configured location
    * ways to add
      * | SEPARATE `catalog.rules` key or
        * 👀if it's present -> replace others 👀
          * _Example:_ configuration / reject ANY kind of entities

            ```yaml
            catalog:
              rules: []
            ```

      * | statically configured locations
        * == `catalog.locations[*].rules`

          ```yaml
          catalog:
            rules:
              - allow: [Component, API, Location, Template]
    
            locations:
              - type: url
                target: https://github.com/org/example/blob/master/org-data.yaml
                rules:
                  - allow: [Group]
          ```

## Readonly mode

* TODO:
Processors provide a good way to automate the ingestion of entities when combined
with [Static Location Configuration](#static-location-configuration) or a
discovery processor like
[GitHub Discovery](../../integrations/github/discovery.md). To enforce the usage of
processors to locate entities we can configure the catalog into `readonly` mode.
This configuration disables registering and deleting locations with the catalog APIs.

```yaml
catalog:
  readonly: true
```

> **Note that any plugin relying on the catalog API for creating, updating, and
> deleting entities will not work in this mode.**

Deleting an entity by UUID, `DELETE /entities/by-uid/:uid`, is allowed when using this mode. It may be rediscovered as noted in [explicit deletion](life-of-an-entity.md#explicit-deletion).

A common use case for this configuration is when organizations have a remote
source that should be mirrored into Backstage. To make Backstage a mirror of
this remote source, users cannot also register new entities with e.g. the
[catalog-import](https://github.com/backstage/backstage/tree/master/plugins/catalog-import)
plugin.

## Clean up orphaned entities

In short, entities can become orphaned through multiple means, such as when a catalog-info YAML file is moved from one place to another in the version control system without updating the registration in the catalog. For safety reasons, the default behavior is to just tag the orphaned entities, and keep them around. You can read more about orphaned entities [here](life-of-an-entity.md#orphaning).

However, if you do wish to automatically remove the orphaned entities, you can use the following configuration, and everything with an orphaned entity tag will be eventually deleted.

```yaml
catalog:
  orphanStrategy: delete
```

## Processing Interval

The [processing loop](./life-of-an-entity.md#processing) is
responsible for running your registered processors on all entities, on a certain
interval. That interval can be configured with the `processingInterval`
app-config parameter.

```yaml
catalog:
  processingInterval: { minutes: 45 }
```

The value is a duration object, that has one or more of the fields `years`,
`months`, `weeks`, `days`, `hours`, `minutes`, `seconds`, and `milliseconds`.
You can combine them, for example as `{ hours: 1, minutes: 15 }` which
essentially means that you want the processing loop to visit entities roughly
once every 75 minutes.

Note that this is only a suggested minimum, and the actual interval may be
longer. Internally, the catalog will scale up this number by a small factor and
choose random numbers in that range to spread out the load. If the catalog is
overloaded and cannot process all entities during the interval, the time taken
between processing runs of any given entity may also be longer than specified
here.

Setting this value too low risks exhausting rate limits on external systems that
are queried by processors, such as version control systems housing catalog-info
files.

## Stitching strategy

[Stitching](./life-of-an-entity.md#stitching) finalizes the entity. It can be run in
two modes:

- `immediate` - performs stitching in-band immediately when needed
- `deferred` - performs the stitching asynchronously

It can be configured with the `stitchingStrategy` app-config parameter.

```yaml title="app-config.yaml"
catalog:
  stitchingStrategy: immediate
```

For the `deferred` mode you can set up additional parameters to further tune the process,
by setting the following parameters:

- `pollingInterval` - the interval between polling for entities that need stitching
- `stitchTimeout` - the maximum time to wait for an entity to be stitched

These parameters accept a duration object, similar to the `processingInterval` parameter.

```yaml title="app-config.yaml"
catalog:
  stitchingStrategy: deferred
  pollingInterval: { seconds: 1 }
  stitchTimeout: { minutes: 1 };
```

## Subscribing to Catalog Errors

Catalog errors are published to the [events plugin](https://github.com/backstage/backstage/tree/master/plugins/events-node): `@backstage/plugin-events-node`. You can subscribe to events and respond to errors, for example you may wish to log them.

The first step is to add the events backend plugin to your Backstage application. Navigate to your Backstage application directory and add the plugin package.

```bash title="From your Backstage root directory"
yarn --cwd packages/backend add @backstage/plugin-events-backend
```

Now you can install the events backend plugin in your backend.

```ts title="packages/backend/src/index.ts"
backend.add(import('@backstage/plugin-events-backend'));
```

### Logging Errors

If you want to log catalog errors you can install the `@backstage/plugin-catalog-backend-module-logs` module.

Install the catalog logs module.

```bash title="From your Backstage root directory"
yarn --cwd packages/backend add @backstage/plugin-catalog-backend-module-logs
```

Add the module to your backend.

```ts title="packages/backend/src/index.ts"
backend.add(import('@backstage/plugin-catalog-backend-module-logs'));
```

This will log errors with a level of `warn`.

You should now see logs as the catalog emits events. Example:

```
[1] 2024-06-07T00:00:28.787Z events warn Policy check failed for user:default/guest; caused by Error: Malformed envelope, /metadata/tags must be array entity=user:default/guest location=file:/Users/foobar/code/backstage-demo-instance/examples/org.yaml
```

### Custom Error Handling

If you wish to handle catalog errors with specific logic different from logging the errors the following should help you get started. For example, you may wish to send a notification or create a ticket for someone to investigate.

Create a backend module that subscribes to the catalog error events. The topic is `experimental.catalog.errors`.

```ts title="packages/backend/src/index.ts"
import { CATALOG_ERRORS_TOPIC } from '@backstage/plugin-catalog-backend';
import {
  coreServices,
  createBackendModule,
} from '@backstage/backend-plugin-api';
import { eventsServiceRef, EventParams } from '@backstage/plugin-events-node';

interface EventsPayload {
  entity: string;
  location?: string;
  errors: Error[];
}

interface EventsParamsWithPayload extends EventParams {
  eventPayload: EventsPayload;
}

const eventsModuleCatalogErrors = createBackendModule({
  pluginId: 'events',
  moduleId: 'catalog-errors',
  register(env) {
    env.registerInit({
      deps: {
        events: eventsServiceRef,
        logger: coreServices.logger,
      },
      async init({ events, logger }) {
        events.subscribe({
          id: 'catalog',
          topics: [CATALOG_ERRORS_TOPIC],
          async onEvent(params: EventParams): Promise<void> {
            const event = params as EventsParamsWithPayload;
            const { entity, location, errors } = event.eventPayload;
            // Add custom logic here for responding to errors
            for (const error of errors) {
              logger.warn(error.message, {
                entity,
                location,
              });
            }
          },
        });
      },
    });
  },
});
```

Now install your module.

```ts title="packages/backend/src/index.ts"
backend.add(eventsModuleCatalogErrors);
```
