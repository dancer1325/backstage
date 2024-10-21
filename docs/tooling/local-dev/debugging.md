---
id: debugging
title: Debugging Backstage
description: How logging works and how to configure it
---

* use cases to debug it
  * configure Backstage locally
  * contributing
* goal
  * change the logging levels
* Backstage logging
  * 👀-- based on -- [Winston logging library](https://github.com/winstonjs/winston) 👀
    * supports
      * [`npm` logging levels](https://github.com/winstonjs/winston#logging-levels)
        * _Example:_ `warn`, `info` (the default), `debug`
  * instance -- is passed to -- plugins
* way to adjust the logging level
  * set the `LOG_LEVEL` environment variable
    * _Example:_ debug level | running the app locally

    ```shell
    LOG_LEVEL=debug yarn dev
    ```

    The resulting log should now have more information available for debugging:

  ```text
  [1] 2023-04-12T00:51:42.468Z catalog debug Skipped stitching of domain:default/artists, no changes type=plugin
  [1] 2023-04-12T00:51:42.469Z catalog debug Skipped stitching of domain:default/playback, no changes type=plugin
  [1] 2023-04-12T00:51:42.470Z catalog debug Processing system:default/podcast type=plugin
  [1] 2023-04-12T00:51:42.470Z catalog debug Processing group:default/infrastructure type=plugin
  [1] 2023-04-12T00:51:42.470Z catalog debug Processing group:default/boxoffice type=plugin
  [1] 2023-04-12T00:51:42.470Z catalog debug Processing group:default/backstage type=plugin
  [1] 2023-04-12T00:51:42.470Z catalog debug Processing group:default/team-a type=plugin
  [1] 2023-04-12T00:51:42.519Z catalog debug Skipped stitching of group:default/acme-corp, no changes type=plugin
  [1] 2023-04-12T00:51:42.520Z catalog debug Skipped stitching of group:default/backstage, no changes type=plugin
  [1] 2023-04-12T00:51:42.521Z catalog debug Skipped stitching of group:default/boxoffice, no changes type=plugin
  [1] 2023-04-12T00:51:42.523Z catalog debug Processing user:default/breanna.davison type=plugin
  [1] 2023-04-12T00:51:42.524Z catalog debug Processing user:default/janelle.dawe type=plugin
  [1] 2023-04-12T00:51:42.524Z catalog debug Processing user:default/nigel.manning type=plugin
  [1] 2023-04-12T00:51:42.524Z catalog debug Processing user:default/guest type=plugin
  [1] 2023-04-12T00:51:42.525Z catalog debug Processing group:default/team-b type=plugin
  [1] 2023-04-12T00:51:44.057Z search info Starting collation of explore tools type=plugin
  [1] 2023-04-12T00:51:44.095Z backstage info ::1 - - [12/Apr/2023:00:51:44 +0000] "GET /api/explore/tools HTTP/1.1" 200 - "-" "node-fetch/1.0 (+https://github.com/bitinn/node-fetch)" type=incomingRequest
  [1] 2023-04-12T00:51:44.100Z backstage info ::1 - - [12/Apr/2023:00:51:44 +0000] "GET /api/catalog/entities?filter=metadata.annotations.backstage.io%2Ftechdocs-ref&fields=kind,namespace,metadata.annotations,metadata.name,metadata.title,metadata.namespace,spec.type,spec.lifecycle,relations&offset=0&limit=500 HTTP/1.1" 200 - "-" "node-fetch/1.0 (+https://github.com/bitinn/node-fetch)" type=incomingRequest
  [1] 2023-04-12T00:51:44.104Z search info Finished collation of explore tools type=plugin
  [1] 2023-04-12T00:51:44.118Z search info Collating documents for tools succeeded type=plugin documentType=tools
  [1] 2023-04-12T00:51:44.119Z backstage debug task: search_index_tools will next occur around 2023-04-11T21:01:44.118-04:00 type=taskManager task=search_index_tools
  ```

## Debugger

* enable debug functionality / IDE

### VSCode

* TODO:
In your `launch.json`, add a new entry with the following,

```jsonc
{
    "name": "Start Backend",
    "type": "node",
    "request": "launch",
    "args": [
        "package",
        "start"
    ],
    "cwd": "${workspaceFolder}/packages/backend",
    "program": "${workspaceFolder}/node_modules/.bin/backstage-cli",
    "skipFiles": [
        "<node_internals>/**"
    ],
    "console": "integratedTerminal"
},
```

### WebStorm

* Run configurations -- enable the use of -- debugging functionality
  * _Example of debugging functionalities:_ steppers and breakpoints
* steps
  * Select `Edit Configurations` | `Run` dropdown menu & click the "+" sign to add a new
   configuration, then select `Node.js`.
  * input `{PROJECT_DIR}/packages/backend` | `Working directory`
    * `{PROJECT_DIR}` -- must be replaced with the -- path to your Backstage repo
  * input `{PROJECT_DIR}/node_modules/@backstage/cli/bin/backstage-cli` | `JavaScript file`
    * `{PROJECT_DIR}` -- must be replaced with the -- path to your Backstage repo
  * input `package start` | `Application parameters`
  * input `LOG_LEVEL=debug` | `Environment Variables`
    * optionally
  * Click `Apply` to save the changes.
  * `Run` or `Debug` icons | toolbar
