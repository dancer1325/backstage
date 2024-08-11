---
id: architecture-overview
title: Architecture overview
description: Documentation on Architecture overview
---

## Terminology

* _Example:_ Architecture of basic Backstage application

    ![The architecture of a basic Backstage application](../../assets/architecture-overview/backstage-typical-architecture.png)

  * core Backstage UI
    * == wrapper client-side thin / -- around -- a set of plugins

    ![UI with different components highlighted](../../assets/architecture-overview/core-vs-plugin-components-highlighted.png)
  * UI plugins (CircleCI, Catalog, Lighthouse, Tech Radar) + backing services
  * DDBB

* Core
  * == base functionality
* App
  * == instance of a Backstage app /
    * ties Core -- with -- Plugins 
* Plugins
  * := client side applications /
    * mount | UI
    * written in TS or JS
    * stored under ["plugins/"](https://github.com/backstage/backstage/tree/master/plugins)
      * _Example:_ catalog plugin | [plugins/catalog](https://github.com/backstage/backstage/tree/master/plugins/catalog)
  * == additional functionality
  * "Plugin Extension Components" or "Extension Components"
    * := components / -- are exported from -- plugins
    * `create*Extension` to create them -- Check [composability documentation](../../plugins/composability.md) --
    * == React components + metadata / wire together the entire app
  * types
    * company-specific -- _Example:_ 100+ in Spotify --
    * shared by the community
  * how to install in your Backstage application?
    * as React components -- [Example](https://github.com/backstage/backstage/blob/master/packages/app/src/App.tsx#L122) --
  * -- are available in -- UI | dedicated URL
    * URL can be customized
    * _Example:_ `/lighthouse` & `/circlei`

    ![The lighthouse plugin UI](../../assets/architecture-overview/lighthouse-plugin.png)

    ![CircleCI Plugin UI](../../assets/architecture-overview/circle-ci.png)
  * type of architectures
    * standalone
      * := run entirely in the browser
        * == NO API requests to others
      * _Example:_ [Tech Radar Plugin](https://demo.backstage.io/tech-radar), with it's architecture
  
        ![tech radar plugin ui](../../assets/architecture-overview/tech-radar-plugin.png)
    
        ![ui and tech radar plugin connected together - architecture](../../assets/architecture-overview/tech-radar-plugin-architecture.png)

    * service backed
      * := plugins / -- make API requests to -- service running | same organization
      * _Example1:_ Lighthouse plugin
        * -- makes requests to the -- [lighthouse-audit-service](https://github.com/spotify/lighthouse-audit-service) 
      
          ![lighthouse plugin backed to microservice and database](../../assets/architecture-overview/lighthouse-plugin-architecture.png)

      * _Example2:_ software catalog
    * third-party backed
      * := plugins / make API requests to a service running 👁️OUTSIDE 👁️ the organization
        * -> proxy is required
      * _Example:_ [CircleCI](https://circleci.com)

        ![CircleCI plugin talking to proxy talking to SaaS Circle CI](../../assets/architecture-overview/circle-ci-plugin-architecture.png)

* 🧠Reason of these 3 parts 🧠
  * 3 groups of contributors

## Package Architecture

* -- rely on -- NPM packages
* Recommended patterns to follow / set up sound project structure

    ![Package architecture](../../assets/architecture-overview/package-architecture.drawio.svg)

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
  * ALL packages | SAME plugin == common prefix + 1! distinct suffix
    * prefix pattern
      * `@<scope>/plugin-<plugin-id>`
      * `backstage-plugin-<plugin-id>`
      * `@<scope>/backstage-plugin-<plugin-id>`
    * suffix denotes the role -- Check [Plugin Package Structure ADR](../../architecture-decisions/adr011-plugin-package-structure.md) --
  * _Example:_ 2 FE packages + 2 BE packages + 1 isomorphic package 
  * additional FE or BE modules
    * optional
  * 's external library
    * == `-react` + `-common` + `-node` plugin packages
    * enables OTHER plugins | top
      * -> plugin1 -- communicate via library with -- plugin2
    * recommendations
      * allow duplicate installations of themselves
        * Reason: 🧠 mix of versions / -- installed as dependencies of -- various plugins  🧠
* FE packages
  * groups
    * FE App core
      * := packages / allows
        * build up "app" package
        * foundation for the plugin libraries
    * FE Plugin Core + FE libraries
      * FE Plugin Core
        * pretty stable
        * -- part of -- FE framework
      * FE libraries
        * == building blocks / create the plugins 
* BE packages
  * == building blocks + patterns /
    * uses
      * build BE services
  * 's architecture != FE packages / -- based on - plugins
* Common packages
  * := isomorphic packages /
    * == executable either FE or BE
    * -- NOT depend on other -- FE or BE package 
    * small set
* Where to place your code?
  * principles to follow
    * exposure of your code as low as possible

        ![Package decision](../../assets/architecture-overview/package-decision.drawio.svg)

## Databases

* TODO: 
As we have seen, both the `lighthouse-audit-service` and `catalog-backend`
require a database to work with.

The Backstage backend and its built-in plugins are based on the
[Knex](http://knexjs.org/) library, and set up a separate logical database per
plugin. This gives great isolation and lets them perform migrations and evolve
separate from each other.

The Knex library supports a multitude of databases, but Backstage is at the time
of writing tested primarily against two of them: SQLite, which is mainly used as
an in-memory mock/test database, and PostgreSQL, which is the preferred
production database. Other databases such as the MySQL variants are reported to
work but
[aren't tested as fully](https://github.com/backstage/backstage/issues/2460)
yet.

## Cache

The Backstage backend and its built-in plugins are also able to leverage cache
stores as a means of improving performance or reliability. Similar to how
databases are supported, plugins receive logically separated cache connections,
which are powered by [Keyv](https://github.com/lukechilds/keyv) under the hood.

At this time of writing, Backstage can be configured to use one of three cache
stores: memory, which is mainly used for local testing, memcache or Redis,
which are cache stores better suited for production deployment. The right cache
store for your Backstage instance will depend on your own run-time constraints
and those required of the plugins you're running.

### Use memory for cache

```yaml
backend:
  cache:
    store: memory
```

### Use memcache for cache

```yaml
backend:
  cache:
    store: memcache
    connection: user:pass@cache.example.com:11211
```

### Use Redis for cache

```yaml
backend:
  cache:
    store: redis
    connection: redis://user:pass@cache.example.com:6379
    useRedisSets: true
```

The useRedisSets flag is explained [here](https://github.com/jaredwray/keyv/tree/main/packages/redis#useredissets).

Contributions supporting other cache stores are welcome!

## Containerization

The example Backstage architecture shown above would Dockerize into three
separate Docker images.

1. The frontend container
2. The backend container
3. The Lighthouse audit service container

![Boxes around the architecture to indicate how it is containerised](../../assets/architecture-overview/containerised.png)

The backend container can be built by running the following command:

```bash
yarn run build
yarn run build-image
```

This will create a container called `example-backend`.

The lighthouse-audit-service container is already publicly available in Docker
Hub and can be downloaded and run with

```bash
docker run spotify/lighthouse-audit-service:latest
```
