---
id: software-templates-index
title: Backstage Software Templates
sidebar_label: Overview
# prettier-ignore
description: The Software Templates part of Backstage is a tool that can help you create Components inside Backstage
---

* Software Templates
  * == tool / -- can help you create -- Components | Backstage
    * load skeletons of code / template in some variables
    * publish the template to some locations (_Example:_ GitHub or GitLab)

<video width="100%" height="100%" controls>
  <source src="/video/software-templates.mp4" type="video/mp4" />
</video>

## Getting Started

* check before[Getting Started with Backstage](../../getting-started)
* requirements
  * if you're running Backstage with Node v20+ -> 
    * pass `--no-node-snapshot` to Node command or
    * `export NODE_OPTIONS=--no-node-snapshot`
      * == specify the `NODE_OPTIONS` environment variable
* available | `/create`
  * _Example:_ `http://localhost:3000/create`

  ![Create Image](../../assets/software-templates/create.png)

## Choose a template

* each template -- can ask for -- different input variables

  ![Enter some variables](../../assets/software-templates/template-picked.png)

* required fields for Backstage usage
  * owner 
    * == `user` | backstage system
  * `storePath`
    * == destination URL -- to create for the --provider
      * _Example:_ `https://github.com/backstage/my-new-repository`, `https://gitlab.com/myorg/myrepo`

  ![Enter Backstage vars](../../assets/software-templates/template-picked-2.png)

## Run!

* once you've entered values and confirmed -> get a popup box / live progress of what is currently happening

  ![Templating Running](../../assets/software-templates/running.png)

  * if all goes well -> you get a success screen!

  ![Templating Complete](../../assets/software-templates/complete.png)

  * if it fails -> get the log / each section

  ![Templating failed](../../assets/software-templates/failed.png)

## View Component in Catalog

* once it's been created -> redirected to the `View in Catalog` button

  ![Catalog](../../assets/software-templates/go-to-catalog.png)

* | Catalog View table

  ![Catalog](../../assets/software-templates/added-to-the-catalog-list.png)

## Disable Register Existing Component button

* == disable the default route binding | `scaffolderPlugin.registerComponent`
* ways
  * | `backstage/packages/app/src/App.tsx`:

  ```diff
   const app = createApp({
     apis,
     bindRoutes({ bind }) {
       bind(scaffolderPlugin.externalRoutes, {
  +      registerComponent: false,
  -      registerComponent: catalogImportPlugin.routes.importPage,
         viewTechDoc: techdocsPlugin.routes.docRoot,
       });
  })
  ```

  * | `app-config.yaml`

  ```yaml
  app:
    routes:
      bindings:
        scaffolder.registerComponent: false
  ```

## Previewing and Executing Previous Template Tasks

* execution of a template
  * == unique task / unique ID
  * list of previously executed template tasks
    * | "Create" page, "Task List"

    ![Template Task List](../../assets/software-templates/template-task-list.png)

  * if you want to re-run ->  navigate to the template tasks page. Locate the desired task and select the "Start Over" option from the context menu.

    ![Template Start Over](../../assets/software-templates/template-start-over.png)
