* Main parts
  * Core
    * == base functionality
  * App
    * == instance of a Backstage app /
      * Core -- is tied with -- Plugins
  * Plugins
    * := client side applications / 
      * mount on the UI
      * written in TS or JS
      * stored under ["plugins/"](https://github.com/backstage/backstage/tree/master/plugins)
    * how to install in your Backstage application?
      * as React components -- [Example](https://github.com/backstage/backstage/blob/master/packages/app/src/App.tsx#L122) --
    * -- are available in -- UI | dedicated URL 
      * _Example:_ `/lighthouse` & `/circlei` 
    * type of architectures
      * standalone
        * := run entirely in the browser
          * == NO API requests to others
        * _Example:_ [Tech Radar Plugin](https://demo.backstage.io/tech-radar), check "techRadarPluginArchitecture.png"
      * service backed
        * := plugins / make API requests to a service running into the same organization
        * _Example:_ Lighthouse plugin, software catalog
      * third-party backed
        * := plugins / make API requests to a service running 👁️OUTSIDE 👁️ the organization
          * -> proxy is required
        * _Example:_ [CircleCI](https://circleci.com)
    * types
      * company-specific -- _Example:_ 100+ in Spotify --
      * shared by the community
* _Example:_ Check "exampleMainParts.png"
  * core Backstage UI
    * == wrapper client-side thin / -- around -- a set of plugins 
  * UI plugins (CircleCI, Catalog, Lighthouse, Tech Radar) + backing services
  * DDBB

## Package architecture
* -- rely on -- NPM packages
* Recommended patterns to follow -- 'backstagePackageArchitecture.png' --
  * arrows
    * == runtime dependency -- NO `devDependencies` displayed --
  * entry points of Backstage project
    * "app" package
      * := FE application / brings FE plugins customized for your organization
      * recommended 1! instance
    * "backend" package
      * == backend service / powers Backstage application
      * recommended 1! instance
* plugin packages
  * == several packages + additional FE or BE modules
    * packages
      * share common prefix + 1! distinct suffix
        * prefix pattern
          * `@<scope>/plugin-<plugin-id>`
          * `backstage-plugin-<plugin-id>`
          * `@<scope>/backstage-plugin-<plugin-id>`
      * _Example:_ 2 FE packages + 2 BE packages + 1 isomorphic package
    * additional FE or BE modules
      * optional
* FE packages
  * groups
    * FE App core
      * := packages / used by "app" package
    * FE Plugin Core + FE libraries
      * FE libraries
        * == building blocks / 
          * uses
            * creating plugins
* BE packages
  * == building blocks /
    * uses
      * build BE services
* Common packages
  * := isomorphic packages
    * == executable either FE or BE
* Where to place your code?
  * Check 'whereToPlaceCodeDecisionTree.png'
* TODO:


## DDBB
* [Knex](https://knexjs.org)
  * := library
    * uses
      * backstage BE & built-in plugins
    * allows
      * separated logical DDBB / plugin
    * supports multitude of DDBB
* Supported by Backstage
  * SQLite
    * uses
      * in-memory mock/test DDBB
  * PostgreSQL
    * recommended to use in production

## Cache
* Uses
  * Backstage BE & built-in plugins
    * via [Keyv](https://github.com/jaredwray/keyv)
* Supported by Backstage
  * memory
    * uses
      * local testing
      ```
       backend:
         cache:
           store: memory
      ```
  * memcache
      ```
      backend:
        cache:
          store: memcache
          connection: user:pass@cache.example.com:11211
      ```
  * Redis
      ```
      backend:
        cache:
          store: redis
          connection: redis://user:pass@cache.example.com:6379
          useRedisSets: true
      ```


## Containerization
* TODO: What example above?
