# Backstage Catalog Frontend

* `@backstage/plugin-catalog` package
* == React frontend -- for the -- default Backstage [software catalog](http://backstage.io/docs/features/software-catalog/) /
  * provides
    * interfaces -- related to --
      * listing catalog entities
      * other information about them | entity pages

## Installation

* if Backstage application is created with `npx @backstage/create-app` -> installed by default
  * == NOT required installation
* this package is | `packages/app/package.json` 's `dependencies` block
  * check if it's here

### Install the package

* 👀check previous notes, to check if it's ALREADY included 👀
* steps
  ```bash
  # From your Backstage root directory
  yarn --cwd packages/app add @backstage/plugin-catalog
  ```

### Add the plugin | your `packages/app`

* add the 2 pages / catalog plugin -- provides to -- your app
  * route naming
    * choose whatever you want
    * recommendations

      ```diff
      // packages/app/src/App.tsx
      import {
        CatalogIndexPage,
        CatalogEntityPage,
      } from '@backstage/plugin-catalog';
      import { entityPage } from './components/catalog/EntityPage';
      
      <FlatRoutes>
      +  <Route path="/catalog" element={<CatalogIndexPage />} />
      +  <Route path="/catalog/:namespace/:kind/:name" element={<CatalogEntityPage />}>
      +  {/*
      +    This is the root of the custom entity pages for your app, refer to the example app
      +    in the main repo or the output of @backstage/create-app for an example
      +  */}
      +    {entityPage}
      +  </Route>
        ...
      </FlatRoutes>
      ```

* `createComponent`
  * == external route / -- should be linked to the -- page | user -- can create -- components
    * _Example:_ -- typically linked to the -- scaffolder plugin's template index page

  ```diff
  // packages/app/src/App.tsx
  +import { catalogPlugin } from '@backstage/plugin-catalog';
  +import { scaffolderPlugin } from '@backstage/plugin-scaffolder';
    
  const app = createApp({
    // ...
    bindRoutes({ bind }) {
  +    bind(catalogPlugin.externalRoutes, {
  +      createComponent: scaffolderPlugin.routes.root,
  +    });
    },
  });
  ```

* add a link -- to the -- catalog index page | your application sidebar

  ```diff
  // packages/app/src/components/Root/Root.tsx
  +import HomeIcon from '@material-ui/icons/Home';
  
  export const Root = ({ children }: PropsWithChildren<{}>) => (
    <SidebarPage>
      <Sidebar>
  +      <SidebarItem icon={HomeIcon} to="catalog" text="Home" />
        ...
      </Sidebar>
  ```

## Development

* `yarn start` | own package
  * allows
    * ⭐️starting in a standalone mode ⭐️
      * -> limited functionality and that
      * uses
        * developing the catalog FE plugin itself
* | root folder
  * 
    ```bash
    yarn dev
    ```
    * == run the entire Backstage example application
      * == FE + Backend / -- populated with -- SOME example entities

## Links

* [catalog-backend](https://github.com/backstage/backstage/tree/master/plugins/catalog-backend)
  * == backend API -- for -- this frontend
