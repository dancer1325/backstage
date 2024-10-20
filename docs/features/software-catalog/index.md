---
id: software-catalog-overview
title: Backstage Software Catalog
sidebar_label: Overview
# prettier-ignore
description: The Backstage Software Catalog
---

## What is a Software Catalog?

* := centralized system /
  * keeps track for ALL the software (services, websites, libraries, data pipelines, ..) | your ecosystem 
    * ownership
    * metadata 
  * == [metadata YAML files](descriptor-format.md) / stored together with the code
    * -> | source control (GitHub, GitHub Enterprise, GitLab, ...)
    * recommendations
      * | repository rot

![software-catalog](../../assets/header.png)

* use cases
  * manage and maintain the software of ALL Teams
  * makes the software discoverable | your company
* available | `/catalog`
  * _Example:_ in [Getting Started with Backstage](../../getting-started) -> available | `http://localhost:3000`

![screenshot of software catalog](../../assets/software-catalog/software-catalog-home.png)

* ways to add components to the catalog
  * register components manually (== via UI)
    * clicking the **REGISTER EXISTING COMPONENT** | `/create`
    ![screenshot of manually register existing component](../../assets/software-catalog/bsc-register-1.png)
      * Backstage -- expects the -- full URL to the YAML in your source control
        * _Example:_
        ```bash
        https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/components/artist-lookup-component.yaml
        ```
        * _More examples:_ Check [here](https://github.com/backstage/backstage/tree/master/packages/catalog-model/examples)

    ![screenshot of creating new components](../../assets/software-catalog/bsc-register-2.png)
    * 👁️ANY kind of software can be registered 👁️
      * even software / NOT maintained by your company -- _Example:_ SaaS offering --
  * creating new components -- through -- Backstage
    * TODO:
  * -- integrating with an -- [external source](external-integrations.md)

### Creating new components through Backstage

* ALL software -- created through the -- [Backstage Software Templates](../software-templates/index.md) -> are automatically
registered | catalog

### Static catalog configuration

* alternative to 
  * manually registering components
* [static configuration](../../conf/index.md)
* _Example:_

  ```yaml
  catalog:
    locations:
      - type: url
        target: https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/components/artist-lookup-component.yaml
  ```
* [catalog configuration information](configuration.md)

### Updating component metadata

* TODO:
Teams owning the components are responsible for maintaining the metadata about
them, and do so using their normal Git workflow.

![screenshot of updating component metadata](../../assets/software-catalog/bsc-edit.png)

Once the change has been merged, Backstage will automatically show the updated
metadata in the software catalog after a short while.

## Finding software in the catalog

By default the software catalog shows components owned by the team of the logged
in user. But you can also switch to _All_ to see all the components across your
company's software ecosystem. Basic inline _search_ and _column filtering_ makes
it easy to browse a big set of components.

![screenshot of finding software in the catalog](../../assets/software-catalog/bsc-search.png)

## Starring components

For easy and quick access to components you visit frequently, Backstage supports
_starring_ of components:

![screenshot of starred components](../../assets/software-catalog/bsc-starred.png)

## Integrated tooling through plugins

The software catalog is a great way to organize the infrastructure tools you use
to manage the software. This is how Backstage creates one developer portal for
all your tools. Rather than asking teams to jump between different
infrastructure UIs (and incurring additional cognitive overhead each time they
make a context switch), most of these tools can be organized around the entities
in the catalog.

![screenshot of tools](https://backstage.io/assets/images/tabs-abfdf72185d3ceb1d92c4237f7f78809.png)

Your Backstage developer portal can be customized by incorporating
[existing open source plugins](https://github.com/backstage/backstage/tree/master/plugins),
or by [building your own](../../plugins/index.md).

## Links

- [[Blog post] Backstage Service Catalog released in alpha](https://backstage.io/blog/2020/06/22/backstage-service-catalog-alpha)
