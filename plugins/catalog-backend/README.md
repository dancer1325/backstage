# Catalog Backend

* `@backstage/plugin-catalog-backend` package
* == backend -- for the -- default Backstage [software catalog](http://backstage.io/docs/features/software-catalog/) /
  * provides
    * API -- for -- consumers
      * _Example:_ [FE catalog plugin](https://github.com/backstage/backstage/tree/master/plugins/catalog)
    * builtin database-backed implementation -- of the -- catalog / 
      * can
        * store
        * serve your catalog for you
  * uses
    * as a bridge -- to your -- existing catalog solutions
      * ingesting data / store | database
      * proxying calls -- to an -- external catalog service

## Installation

* if Backstage application is created with `npx @backstage/create-app` -> installed by default
  * == NOT required installation
* this package is | `packages/backend/package.json` 's `dependencies` block 
  * check if it's here

### Install the package

* 👀check previous notes, to check if it's ALREADY included 👀
* steps

  ```bash
  # From your Backstage root directory
  yarn --cwd packages/backend add @backstage/plugin-catalog-backend
  ```

  ```ts, fileName=packages/backend/src/index.ts
  const backend = createBackend();
  // ...
  backend.add(import('@backstage/plugin-catalog-backend/alpha'));
  ```

#### Old backend system

* TODO:
In the old backend system there's a bit more wiring required. You'll need to
create a file called `packages/backend/src/plugins/catalog.ts` with contents
matching [catalog.ts in the create-app template](https://github.com/backstage/backstage/blob/ad9314d3a7e0405719ba93badf96e97adde8ef83/packages/create-app/templates/default-app/packages/backend/src/plugins/catalog.ts).

With the `catalog.ts` router setup in place, add the router to
`packages/backend/src/index.ts`:

```diff
+import catalog from './plugins/catalog';

async function main() {
  ...
  const createEnv = makeCreateEnv(config);

+  const catalogEnv = useHotMemoize(module, () => createEnv('catalog'));
  const scaffolderEnv = useHotMemoize(module, () => createEnv('scaffolder'));

  const apiRouter = Router();
+  apiRouter.use('/catalog', await catalog(catalogEnv));
  ...
  apiRouter.use(notFoundHandler());

```

### Adding catalog entities

* requirements
  * `catalog-backend` installed | your backend package
* catalog entities
  * NO 1 so far
  * if you want to add ->
    * check [Catalog Configuration](https://backstage.io/docs/features/software-catalog/configuration)
    * [copy the catalog locations -- from the -- create-app template](https://github.com/backstage/backstage/blob/master/packages/create-app/templates/default-app/app-config.yaml.hbs)

## Development

* `yarn start` | own package
  * allows
    * ⭐️starting in a standalone mode ⭐️
      * -> limited functionality and that
      * uses
        * developing the catalog backend plugin itself
*
  ```bash
  # in one terminal window, run this from from the very root of the Backstage project
  cd packages/backend
  yarn start
  ``` 
  * == run the entire Backstage example application 
    * == FE + Backend / -- populated with -- SOME example entities
  * allows
    * evaluating the catalog & have a greater amount of functionality

## Links

* [catalog](https://github.com/backstage/backstage/tree/master/plugins/catalog)
  * == ⭐️this plugin's FE interface ⭐️
