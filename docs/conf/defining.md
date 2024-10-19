---
id: defining
title: Defining Configuration for your Plugin
description: Documentation on Defining Configuration for your Plugin
---

* Configuration in Backstage -- is organized via a -- configuration schema /
  * -- is defined via a superset of -- [JSON Schema Draft-07](https://json-schema.org/specification-links.html#draft-7)
  * Each plugin or package | Backstage app -- can contribute to the -- configuration schema

## Schema Collection and Definition

* Schemas -- are collected from -- ALL packages (ALSO the root) + dependencies (ALSO transitive) | EACH repo /
part of the Backstage ecosystem
  * "part of the Backstage ecosystem" == package / has
    * \>=1 dependency | `@backstage` namespace or
    * `"configSchema"` field | `package.json` / can contain
      * inlined JSON schema
      * -- relative path to a -- schema file / supported
        * `.json`
        * `.d.ts`
      * _Example:_ 
      ```jsonc title="package.json"
      {
        // ...
        // CRUTIAL to include the configSchema relative HERE in "files"
        "files": [
          // ...
          "config.d.ts"
        ],
        "configSchema": "config.d.ts"
      }
      ```

      ```ts, name="config.d.ts"
      // 1! `Config` MUST be exported
      export interface Config {
        app: {
          /**
           * Frontend root URL
           * @visibility frontend
           */
          baseUrl: string;
      
          // Use @items.<name> to assign annotations to primitive array items
          /** @items.visibility frontend */
          myItems: string[];
        };
      }
      ```

* Separate `.json` schema files -- can use a -- top-level `"$schema": "https://backstage.io/schema/config-v1"` declaration 
  * -> receive schema
    * validation
    * autocompletion

      ```json
      {
        "$schema": "https://backstage.io/schema/config-v1",
        "type": "object",
        "properties": {
          "app": {
            "type": "object",
            "properties": {
              "baseUrl": {
                "type": "string",
                "description": "Frontend root URL",
                "visibility": "frontend"
              }
            },
            "required": ["baseUrl"]
          },
          "required": ["app"]
        }
      }
      ```

## Visibility

* `https://backstage.io/schema/config-v1` meta schema
  * == superset of JSON Schema Draft 07 / 
    * additions
      * are
        * `visibility`
          * ONLY applies | own field 
          * if you want to set -> `@visibility` | `Config` Typescript interface
        * `deepVisibility`
          * applies | own field + children fields
          * if you want to set -> `@deepVisibility` | `Config` Typescript interface
      * uses
        * indicate whether the given config value -- should be -- visible | frontend or not
      * allowed values
        * `frontend`
        * `backend`
          * default
        * `secret`
          * == scope | runtime
          * if you define `frontend` & `secret` | 2 different schemas -> error | schema merging

    
    | `visibility` |                                                                    |
    | ------------ | ------------------------------------------------------------------ |
    | `frontend`   | Visible in frontend and backend                                    |
    | `backend`    | (Default) Only in backend                                          |
    | `secret`     | Only in backend and may be excluded from logs for security reasons |
    

  ```ts
  export interface Config {
    app: {
      /**
       * Frontend root URL
       * NOTE: Visibility applies to only this field
       * @visibility frontend
       */
      baseUrl: string;
  
      /**
       * Some custom complex type
       * NOTE: Visibility applies recursively downward
       * This is particularly useful for complex types like durations
       * @deepVisibility frontend
       */
      customSchedule: HumanDuration;
    };
  
    backend: {
      /**
       * Some custom credentials type
       * NOTE: Visibility applies recursively downward, and this would NOT have
       * been safe if the regular visibility keyword had been used
       * @deepVisibility secret
       */
      customCredentials: {
        password: string;
      };
    };
  }
  ```

## Validation of the schemas

* -- via -- `backstage-cli config:check`
* if you want to validate the frontend configuration -> use
`backstage-cli config:print --frontend`

## Guidelines

* static configuration
  * NO use it as default
  * wonder if a particular option actually -- needs to be -- static configuration
  * alternative
    * TypeScript API
      * recommended in general
* when you define configuration | your plugin -> keys
  * on `camelCase` form
  * -- stick to -- existing casing conventions
    * _Example:_ `baseUrl` rather than `baseURL`
* objects
  * more recommended over arrays
    * Reason: 🧠 possible to override individual values -- via -- separate files or environment variables 🧠
* avoid creating new top-level fields
