---
id: index
title: Static Configuration in Backstage
description: Documentation on Static Configuration in Backstage
---

## Summary

* Backstage
  * -- ships with a -- flexible configuration system / 
    * simple way to configure | local development & production deployments  
      * Backstage apps
      * Backstage plugins 
    * -> helps get you up & running fast
  * uses
    * == tool for plugin authors / 
      * simple to pick up & install a plugin
      * allows for customization

## Supplying Configuration

* ways to supply configuration
  * stored | YAML files
    * `app-config.yaml`
      * default
    * `app-config.local.yaml`
      * uses
        * local overrides
    * others
      * -- via passing -- `--config <path>`
    * allows
      * loading in data and secrets -- from -- various sources
        * _Example:_ using `$env` & `$file` keys
  * environment variables
    * _Example:_ `APP_CONFIG_app_baseUrl=https://staging.example.com`
    * 👀recommended ocasionally 👀
      * temporary overrides -- during -- development
      * reuse deployment artifacts | different environments
* configuration / common between frontend and backend -> needs to be defined 1! time
  * Reason: 🧠 it's shared 🧠
  * _Example:_ `backend.baseUrl`
* [Writing Configuration](./writing.md)

## Configuration Schema

* == plugin`s pieces + package`s pieces 
* uses
  * select what configuration is available | frontend
    * custom `visibility` keyword is by default ONLY available | backend
* ways to validate your configuration schema 
  * JSON Schema definitions 
  * `backstage-cli config:check`
* [Defining Configuration](./defining.md)
  * the schema is defined -- via --
    * JSON Schema or
    * TypeScript

## Reading Configuration

As a plugin developer, you likely end up wanting to define configuration that
you want users of your plugin to supply, as well as reading that configuration
in frontend and backend plugins. For more details, see
[Reading Configuration](./reading.md) and
[Defining Configuration](./defining.md).

## Further Reading

More details are provided in dedicated sections of the documentation.

- [Reading Configuration](./reading.md): How to read configuration in your
  plugin.
- [Writing Configuration](./writing.md): How to provide configuration for your
  Backstage deployment.
- [Defining Configuration](./defining.md): How to define a configuration schema
  for users of your plugin or package.
