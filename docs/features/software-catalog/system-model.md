---
id: system-model
title: System Model
description: Documentation on System Model
---

* [RFC](https://github.com/backstage/backstage/issues/390)
  * 👁️ some of the concepts are NOT yet supported | Backstage 👁️

## Core Entities

* 3 core entities / model software | Backstage catalogue 
  * **Components**
    * := individual pieces of software
      * _Examples:_ mobile feature, backend service, web site, data pipeline, ...
  * **APIs**
    * := boundaries between different components
  * **Resources**
    * := physical or virtual infrastructure / -- allows operating a -- component | runtime
      * _Examples:_ BBDD, Pub/Sub topics, S3 buckets or CDNs

![](../../assets/software-catalog/software-model-core-entities.drawio.svg)

### Component

* ways to track it
  * source control
  * some existing open source or commercial software
* can
  * implement APIs / other components can consume
  * consume APIs / -- implemented by -- other components
  * depend on components or resources / attached to it | runtime

### API

* == abstraction /
  * allows
    * scaling large software ecosystems -> 
      * 1@ class citizen | Backstage model
      * primary way to discover existing functionality | ecosystem
  * -- implemented by -- components
  * ways to define them
    * RPC IDL (e.g., Protobuf, GraphQL, ...),
    * a data schema (e.g., Avro, TFRecord, ...),
    * code interfaces
  * requirements 
    * | known machine-readable format
      * -> possible to build | top, further
        * tooling
        * analysis
  * supported
    * documenting
    * indexing
    * searching
* visibility degrees
  * public
    * == available for ANY other component -- to -- consume
      * 👁️ -> main way interaction between components 👁️
  * restricted
    * == ONLY available | listed set of consumers
  * private
    * == ONLY available | their system

### Resource

* if you model resources + components -> allow
  * visualizing resource footprint,
  * create tooling

## Organizational Entities

### User

* describes a person
  * *Example:* employee, contractor

### Group

* describes an organizational entity
  * _Example:_ team, business unit, loose collection of people | interest group

## Ecosystem Modeling

* extra concepts -- to categorize -- entities
  * **Systems**
    * := collection of entities / -- cooperate to perform -- some function
  * **Domains**
    * allows
      * entities and systems -- are related to -- part of the business
  * characteristics
    * optional
    * uses
      * large catalogue of components + APIs + resources
        * Reason: 🧠 highly granular and hard to understand 🧠

![](../../assets/software-catalog/software-model-entities.drawio.svg)

### System

* TODO:
With increasing complexity in software, systems form an important abstraction
level to help us reason about software ecosystems. Systems are a useful concept
in that they allow us to ignore the implementation details of a certain
functionality for consumers, while allowing the owning team to make changes as
they see fit (leading to low coupling).

A system, in this sense, is a collection of resources and components that
exposes one or several public APIs. The main benefit of modelling a system is
that it hides its resources and private APIs between the components for any
consumers. This means that as the owner, you can evolve the implementation, in
terms of components and resources, without your consumers being able to notice.
Typically, a system will consist of at most a handful of components (see Domain
for a grouping of systems).

For example, a playlist management system might encapsulate a backend service to
update playlists, a backend service to query them, and a database to store them.
It could expose an RPC API, a daily snapshots dataset, and an event stream of
playlist updates.

### Domain

While systems are the basic level of encapsulation for related entities, it is
often useful to group a collection of systems that share terminology, domain
models, metrics, KPIs, business purpose, or documentation, i.e. they form a
bounded context.

For example, it would make sense if the different systems in the “Payments”
domain would come with some documentation on how to accept payments for a new
product or use-case, share the same entity types in their APIs, and integrate
well with each other. Other domains could be “Content Ingestion”, “Ads” or
“Search”.

In case of a large organization, it might make sense to further group domains
in a hierarchy, where a domain can be a subdomain of another domain.

## Other

### Location

A location is a marker that references other places to look for catalog data.

### Type

The type field in the system has no set meaning. It is up to the user to assign their own types and use them as desired, such as for link validation or creating custom UI components. Some common pre-defined types are depicted in the
[ecosystem modeling diagram](#ecosystem-modeling).

### Template

A template definition describes both the parameters that are rendered in the
frontend part of the scaffolding wizard, and the steps that are executed when
scaffolding that component.
