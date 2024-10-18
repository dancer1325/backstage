---
id: overview
title: Overview
description: A high level overview of the Backstage permission framework
---

* [Backstage authentication methods](../auth/index.md)
* goal here
  * Backstage _authorization_
    * == specify data, APIs, or interface actions -- about type of -- allowed access / given user
* Backstage endpoints
  * NOT protected
    * == ALL actions -- are available to -- anyone
    * HOWEVER, it's a common need / many organizations
* permission framework
  * allows
    * protecting Backstage endpoints -- via -- granular permissioning / those resources and actions
  * main design properties
    * Flexibility
      * == configure different authorization methods -- via -- integrators, to
        * role-based access control (RBAC)
        * attribute-based access control (ABAC),
        * bespoke logic expressed in code
        * -- integrations with -- external authorization providers
    * usability
      * -- focus on -- configuring the permission policy
        * Reason: 🧠ALL of its moving parts -- out of the -- box 🧠
        * -- via --
          * integrators
          * plugin authors / -- integrate support for -- permissions

## How does it work?

- **Plugin authors** can add permission support in their plugins by declaring which resources from their plugins can be placed behind authorization and what types of actions users may take upon those resources.

- **Contributors** can implement and share authorization methods (such as RBAC).

- **Integrators** can author or configure policies that define which users can take certain actions upon which resources.

![backstage framework overview](../assets/permissions/permission-framework-overview.drawio.svg)

1. The user triggers a request to perform some action. The request specifies the authorization details using the permission specified by the plugin (in this case, a resource read action).

   - The action may be triggered by a user interacting with the UI, but it can also be a direct request to the plugin's backend.

2. The plugin backend sends a request to the permission framework's backend with the authorization details.

3. The permission framework's backend delegates the authorization decision to the permission policy, which is specified by the integrator using code, a provided authorization method (such as RBAC), or integrations with external authorization providers.

4. An authorization decision is sent to the plugin from the permission backend.

5. The user is either granted access or an error is shown. The plugin is responsible for implementing a response to the user.

## How do I get started?

* [getting started](./getting-started.md)
* if you are a plugin author -> [documentation for plugin authors](plugin-authors/01-setup.md)
