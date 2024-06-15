# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

## Prerequisites
* Check [gettingStarted](https://backstage.io/docs/getting-started/#prerequisites)

## How has it been created?
* `npx @backstage/create-app@latest`

## How to run?
* `yarn install`
  * it can take time!!
* `yarn dev`
  * Opened in your browser automatically or go to 'localhost:3000'

## Configure a PostgreSQL Database
* `yarn --cwd packages/backend add pg`
  * install PostgreSQL client
* Follow '../gettingStarted/configuringBackstage/database'

## Notes
* Structure
  * 'app-config.yaml'
    * == main configuration file for the app!!
  * 'catalog-info.yaml'
    * := catalog entities descriptors
  * 'packages/'
    * additional dependencies -> should be installed in the intended workspaces (!= here)
  * 'packages/app'
    * == fully functioning Backstage FE app
  * 'packages/backend'
    * allows
      * adding additional features
        * [Authentication](https://backstage.io/docs/auth/)
        * [Software Catalog](https://backstage.io/docs/features/software-catalog/)
        * [Software Templates](https://backstage.io/docs/features/software-templates/)
        * [TechDocs](https://backstage.io/docs/features/techdocs/)
