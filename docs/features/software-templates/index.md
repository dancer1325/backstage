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

* TODO:
When it's been created, you'll see the `View in Catalog` button, which will take
you to the registered component in the catalog:

![Catalog](../../assets/software-templates/go-to-catalog.png)

And then you'll also be able to see it in the Catalog View table:

![Catalog](../../assets/software-templates/added-to-the-catalog-list.png)

## Disable Register Existing Component button

There could be situations where you would like to disable the
`Register Existing Component` button for your users. For example:

![Disable Button](../../assets/software-templates/disable-register-existing-component-button.png)

To do so, you will un-register / remove the `catalogImportPlugin.routes.importPage`
from `backstage/packages/app/src/App.tsx`:

```diff
 const app = createApp({
   apis,
   bindRoutes({ bind }) {
-    bind(scaffolderPlugin.externalRoutes, {
-      registerComponent: catalogImportPlugin.routes.importPage,
-    });
     bind(orgPlugin.externalRoutes, {
       catalogIndex: catalogPlugin.routes.catalogIndex,
     });
```

After the change, you should no longer see the button.

## Previewing and Executing Previous Template Tasks

Each execution of a template is treated as a unique task, identifiable by its own unique ID. To view a list of previously executed template tasks, navigate to the "Create" page and access the "Task List" from the context menu (represented by the vertical ellipsis, or 'kebab menu', icon in the upper right corner).

![Template Task List](../../assets/software-templates/template-task-list.png)

If you wish to re-run a previously executed template, navigate to the template tasks page. Locate the desired task and select the "Start Over" option from the context menu.

![Template Start Over](../../assets/software-templates/template-start-over.png)

This action will initiate a new execution of the selected template, pre-populated with the same parameters as the previous run, but these parameters can be edited before re-execution.

In the event of a failed template execution, the "Start Over" option can be used to re-execute the template. The parameters from the original run will be pre-filled, but they can be adjusted as needed before retrying the template.
