---
id: getting-started
title: Getting Started
description: How to get started with the permission framework as an integrator
---

* https://www.youtube.com/embed/EQr9tFClgG0
  * `@backstage/create-app@0.4.14`
  * TODO:

Backstage integrators control permissions by writing a policy. In general terms, a policy is simply an async function which receives a request to authorize a specific action for a user and (optional) resource, and returns a decision on whether to authorize that permission. Integrators can implement their own policies from scratch, or adopt reusable policies written by others.

## Prerequisites

The permissions framework depends on a few other Backstage systems, which must be set up before we can dive into writing a policy.

### Upgrade to the latest version of Backstage

The permissions framework itself is new to Backstage and still evolving quickly. To ensure your version of Backstage has all the latest permission-related functionality, it’s important to upgrade to the latest version. The [Backstage upgrade helper](https://backstage.github.io/upgrade-helper/) is a great tool to help ensure that you’ve made all the necessary changes during the upgrade!

### Enable service-to-service authentication

* Service-to-service authentication
  * allows
    * Backstage backend code -- can verify that a -- given request -- originates from -- elsewhere | Backstage backend 
      * 
  * use cases
    * tasks | search index 
      * _Example:_ collation of catalog entities
  * uses
    * configure | before trying to use the permissions framework
      * Reason: 🧠otherwise, the request should NOT be permission 🧠 
  * [how to set up](../auth/service-to-service-auth.md)

### Supply an identity resolver to populate group membership on sign in

**Note**: If you are working off of an existing Backstage instance, you likely already have some form of an identity resolver set up.

Like many other parts of Backstage, the permissions framework relies on information about group membership. This simplifies authoring policies through the use of groups, rather than requiring each user to be listed in the configuration. Group membership is also often useful for conditional permissions, for example allowing permissions to act on an entity to be granted when a user is a member of a group that owns that entity.

[The IdentityResolver docs](../auth/identity-resolver.md) describe the process for resolving group membership on sign in.

## Optionally add cookie-based authentication

Asset requests initiated by the browser will not include a token in the `Authorization` header. If these requests check authorization through the permission framework, as done in plugins like TechDocs, then you'll need to set up cookie-based authentication. Refer to the ["Authenticate API requests"](https://github.com/backstage/backstage/blob/master/contrib/docs/tutorials/authenticate-api-requests.md) tutorial for a demonstration on how to implement this behavior.

## Integrating the permission framework with your Backstage instance

### 1. Set up the permission backend

The permissions framework uses a new `permission-backend` plugin to accept authorization requests from other plugins across your Backstage instance. The Backstage backend does not include this permission backend by default, so you will need to add it:

1. Add `@backstage/plugin-permission-backend` as a dependency of your Backstage backend:

   ```bash
   # From your Backstage root directory
   yarn --cwd packages/backend add @backstage/plugin-permission-backend
   ```

2. Add the following to a new file, `packages/backend/src/plugins/permission.ts`. This adds the permission-backend router, and configures it with a policy which allows everything.

   ```typescript title="packages/backend/src/plugins/permission.ts"
   import { createRouter } from '@backstage/plugin-permission-backend';
   import {
     AuthorizeResult,
     PolicyDecision,
   } from '@backstage/plugin-permission-common';
   import { PermissionPolicy } from '@backstage/plugin-permission-node';
   import { Router } from 'express';
   import { PluginEnvironment } from '../types';

   class TestPermissionPolicy implements PermissionPolicy {
     async handle(): Promise<PolicyDecision> {
       return { result: AuthorizeResult.ALLOW };
     }
   }

   export default async function createPlugin(
     env: PluginEnvironment,
   ): Promise<Router> {
     return await createRouter({
       config: env.config,
       logger: env.logger,
       discovery: env.discovery,
       policy: new TestPermissionPolicy(),
       identity: env.identity,
     });
   }
   ```

3. Wire up the permission policy in `packages/backend/src/index.ts`. [The index in the example backend](https://github.com/backstage/backstage/blob/master/packages/backend/src/index.ts) shows how to do this. You’ll need to import the module from the previous step, create a plugin environment, and add the router to the express app:

   ```ts title="packages/backend/src/index.ts"
   import proxy from './plugins/proxy';
   import techdocs from './plugins/techdocs';
   import search from './plugins/search';
   /* highlight-add-next-line */
   import permission from './plugins/permission';

   async function main() {
     const techdocsEnv = useHotMemoize(module, () => createEnv('techdocs'));
     const searchEnv = useHotMemoize(module, () => createEnv('search'));
     const appEnv = useHotMemoize(module, () => createEnv('app'));
     /* highlight-add-next-line */
     const permissionEnv = useHotMemoize(module, () => createEnv('permission'));
     // ..

     apiRouter.use('/techdocs', await techdocs(techdocsEnv));
     apiRouter.use('/proxy', await proxy(proxyEnv));
     apiRouter.use('/search', await search(searchEnv));
     /* highlight-add-next-line */
     apiRouter.use('/permission', await permission(permissionEnv));
     // ..
   }
   ```

### 2. Enable and test the permissions system


*
   ```yaml title="app-config.yaml"
   permission:
     enabled: true
   ```

* update the PermissionPolicy | `packages/backend/src/plugins/permission.ts`
  * _Example:_ disable a permission / easy to test -- as -- delete a catalog entity

   ```ts title="packages/backend/src/plugins/permission.ts"
   import { createRouter } from '@backstage/plugin-permission-backend';
   import {
     AuthorizeResult,
     PolicyDecision,
   } from '@backstage/plugin-permission-common';
   /* highlight-remove-next-line */
   import { PermissionPolicy } from '@backstage/plugin-permission-node';
   /* highlight-add-start */
   import {
     PermissionPolicy,
     PolicyQuery,
   } from '@backstage/plugin-permission-node';
   /* highlight-add-end */
   import { Router } from 'express';
   import { PluginEnvironment } from '../types';

   class TestPermissionPolicy implements PermissionPolicy {
     /* highlight-remove-next-line */
     async handle(): Promise<PolicyDecision> {
     /* highlight-add-start */
     async handle(request: PolicyQuery): Promise<PolicyDecision> {
       if (request.permission.name === 'catalog.entity.delete') {
         return {
           result: AuthorizeResult.DENY,
         };
       }
     /* highlight-add-end */

       return { result: AuthorizeResult.ALLOW };
     }
   }
   ```

* check it
  * _Example:_ unregister entity menu option | catalog entity page, is disabled

![Entity detail page showing disabled unregister entity context menu entry](../assets/permissions/disabled-unregister-entity.png)

* 👀framework is fully configured 👀
  * == you can craft a permission policy / works best for your organization -- via --
    * provided authorization method or
    * [writing your own policy](./writing-a-policy.md)
