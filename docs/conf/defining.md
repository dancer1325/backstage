---
id: defining
title: Defining Configuration for your Plugin
description: Documentation on Defining Configuration for your Plugin
---

* Configuration in Backstage -- is organized via a -- configuration schema /
  * -- is defined via a superset of -- [JSON Schema Draft-07](https://json-schema.org/specification-links.html#draft-7)
  * Each plugin or package | Backstage app -- can contribute to the -- configuration schema

## Schema Collection and Definition

* Schemas -- are collected from -- ALL packages (ALSO the root) and dependencies (ALSO transitive) | EACH repo /
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

* TODO:
Separate `.json` schema files can use a top-level
`"$schema": "https://backstage.io/schema/config-v1"` declaration in order to
receive schema validation and autocompletion. For example:

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

The `https://backstage.io/schema/config-v1` meta schema is a superset of JSON
Schema Draft 07. The only additions are the custom `visibility` and
`deepVisibility` keywords, which are used to indicate whether the given config
value should be visible in the frontend or not. The `visibility` marker applies
only to the field it's on, while the `deepVisibility` marker applies to the
field it's on and downwards in the hierarchy as well. The possible values are
`frontend`, `backend`, and `secret`, where `backend` is the default. A
visibility of `secret` has the same scope at runtime, but it will be treated
with more care in certain contexts, and defining both `frontend` and `secret`
for the same value in two different schemas will result in an error during
schema merging.

The visibility only applies to the direct parent of where the keyword is placed
in the schema. For example, if you set the visibility to `frontend` for a subset
of the schema with `type: "object"`, but none of the descendants, only an empty
object will be available in the frontend. The full ancestry does not need to
have correctly defined visibilities however, so it is enough to only for example
declare the visibility of a leaf node of `type: "string"`.

| `visibility` |                                                                    |
| ------------ | ------------------------------------------------------------------ |
| `frontend`   | Visible in frontend and backend                                    |
| `backend`    | (Default) Only in backend                                          |
| `secret`     | Only in backend and may be excluded from logs for security reasons |

You can set visibility with a `@visibility` or `@deepVisibility` comment in the
`Config` Typescript interface.

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

> Make limited use of static configuration. The first question to ask is whether
> a particular option actually needs to be static configuration, or if it might
> just as well be a TypeScript API. In general, options that you want to be able
> to change for different deployment environments should be static
> configuration, while it should otherwise be avoided.

When defining configuration for your plugin, keep keys on `camelCase` form and stick to
existing casing conventions such as `baseUrl` rather than `baseURL`.

It is also usually best to prefer objects over arrays, as it makes it possible
to override individual values using separate files or environment variables.

Avoid creating new top-level fields as much as possible. Either place your
configuration within an existing known top-level block, or create a single new
one using e.g. the name of the product that the plugin integrates.
