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

## Test Permission Policy

To help validate the permission framework is setup we'll create a Test Permission Policy:

1. Backstage ships with a default Allow All Policy, we want to remove that as it would override our Test Permission Policy. To do this remove the following line:

   ```ts title="packages/backend/src/index.ts"
   // permission plugin
   backend.add(import('@backstage/plugin-permission-backend'));
   /* highlight-remove-start */
   backend.add(
     import('@backstage/plugin-permission-backend-module-allow-all-policy'),
   );
   /* highlight-remove-end */
   ```

2. Now we need to add the `@backstage/backend-plugin-api` package:

   ```bash title="from your Backstage root directory"
   yarn --cwd packages/backend add @backstage/backend-plugin-api
   ```

3. Next we will create an `extensions` folder under `packages/backend/src`
4. In this new `extensions` folder we will add a new file called: `permissionsPolicyExtension.ts`
5. Copy the following into the new `permissionsPolicyExtension.ts` file:

   ```ts title="packages/backend/src/extensions/permissionsPolicyExtension.ts"
   import { createBackendModule } from '@backstage/backend-plugin-api';
   import {
     PolicyDecision,
     AuthorizeResult,
   } from '@backstage/plugin-permission-common';
   import { PermissionPolicy } from '@backstage/plugin-permission-node';
   import { policyExtensionPoint } from '@backstage/plugin-permission-node/alpha';

   class TestPermissionPolicy implements PermissionPolicy {
     async handle(): Promise<PolicyDecision> {
       return { result: AuthorizeResult.ALLOW };
     }
   }

   export default createBackendModule({
     pluginId: 'permission',
     moduleId: 'permission-policy',
     register(reg) {
       reg.registerInit({
         deps: { policy: policyExtensionPoint },
         async init({ policy }) {
           policy.setPolicy(new TestPermissionPolicy());
         },
       });
     },
   });
   ```

6. We now need to register this in the backend. We will do this by adding the follow line:

   ```ts title="packages/backend/src/index.ts"
   // permission plugin
   backend.add(import('@backstage/plugin-permission-backend'));
   /* highlight-add-next-line */
   backend.add(import('./extensions/permissionsPolicyExtension'));
   ```

You now have a Test Permission Policy in place, this will help us test that the permission framework is working in the next section.

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
