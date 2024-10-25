---
id: faq
title: Catalog FAQ
sidebar_label: FAQ
description: This page answers frequently asked questions about the catalog
---

This page answers frequently asked questions about the catalog.

## Is it all that important to have users and groups in the catalog at all?

Yes. One of the most important concepts in the catalog is that it exposes your org structure and ownership properly, allowing your users to effectively understand and communicate around your systems. Having catalog entries for users and groups enables end users to navigate around Backstage and click on those owners and being presented with rich information pages around them, instead of getting 404 Not Found pages.

## Can I create users in the catalog on-demand as they sign in?

* types
  * from sign-in interactions /
    * -- based on -- organizational data ( _Example:_ [LDAP](../../integrations/ldap/org.md), [Azure](../../integrations/azure/org.md), bespoke HR systems, etc)
    * check [signing in](../../auth/index.md) 
      * [sign-in resolver](../../auth/identity-resolver.md)
        * translate TP established identity (_Example:_ attributes / returned -- for your -- AD entry) -- into a -- Backstage identity
        * possible to write your own one
    * if they sign in or not -> batch schedule -- ingest -- ALL users and groups
    * requirements
      * `auth` backend
  * on-demand user creation
    * requirements
      * write custom [entity providers](./external-integrations.md)
    * vs previous one
      * more complex
      * maintenance of the custom provider
      * NOT guaranteed to appear immediately
      * WITHOUT complete organizational data -> maybe Backstage does NOT make sense to use
        * _Example1:_ users will NOT be able to check the owners
        * _Example2:_ NOT possible to get an overview of
          * teams ownership or
          * team relations

## Can I call the catalog itself from inside a processor / provider?

* TODO:
Any backend module, including those that provide processors and entity providers to the catalog, are technically able to get hold of a catalog client via the `catalogServiceRef` from `@backstage/plugin-catalog-node`. However, it is almost never the right thing to do - especially from processors - and we strongly discourage from doing so.

The catalog processing loop is a very high-speed system where your entire catalog cluster collaborates to race through all entities at the highest possible rate. The ideal processor does an absolute minimum of work, and immediately relinquishes control back. Performing asynchronous requests to external systems - including the catalog - from processors, can quickly become overwhelming for that external system and starve their resources if they aren't prepared to deal with very high rates of small requests. It also significantly slows down the procesing loop, when each step needs to wait for responses. This can lead to work "piling up" in the catalog and delays in seeing entities get updated. The [life of an entity](./life-of-an-entity.md) article shows the sequence of events that happen when an entity goes from original ingestion, through processing, and to becoming final entities.

See also [the related validation topic](#can-i-validate-relations-in-processors).

## Can I validate relations in processors?

Processors are responsible for generating relations from the entity body - see the [life of an entity](./life-of-an-entity.md) article for more details. It's tempting to put rules in your processors that mark entities as invalid if they have a relation to some other entity that does not exist. For example, a `Component` entity that declares a `spec.owner` to a team that has been disbanded. We strongly discourage from doing this type of "hard" validation in processors, for two reasons.

First, performance. As is described [here](#can-i-call-the-catalog-itself-from-inside-a-processor--provider), you should avoid calling out to the catalog for any reason in processors, including for checking whether a target entity exists. Besides the performance issues, it can also lead to data races where hidden dependencies between entities lead to them never properly settling, or flickering back and forth between states for hard-to-debug reasons.

Second, user experience. The catalog is an eventually consistent system that constantly tries to mirror external realities. Users make changes in catalog-info files, or things update in external systems, and those changes get streamed in and settle over time inside the catalog. But throwing an error in a processor, instantly aborts processing of that entity and stops its ingestion. Now imagine being a large organization where these changes happen maybe hundreds of times per day. Owners of catalog-info files will constantly be surprised by their files "breaking" in ingestion, maybe a very long time after they were initially created - they were valid at their time of creation and haven't been touched since! This is very frustrating and slows down your users because things break silently for reasons out of your control.

There are cases where it's fine to throw hard validation errors in processors. Notably, when it doesn't pass a schema test at all and readers of the catalog data will break if the data was let through. Setting the owner to be a number instead of a string could be such an example.

## Can I throw errors when validating entities?

In short: Sometimes. Only if the shape is so wrong that it would not even parse, or violates TypeScript and similar contracts.

Never throw errors due to "soft" errors, in particular relations not matching an existing target ([see above](#can-i-validate-relations-in-processors)). For soft errors, we recommend instead letting them through into the catalog, and then implementing the checks externally and nudging people gently toward fixing their own metadata. A dynamic info bar at the top of an entity page that informs the owners as they visit the page that a certain relation seems wrong and should be fixed, can go a very long way.

To a large degree, this boils down to user experience. The catalog is an eventually consistent system that constantly tries to mirror external realities. Users make changes in catalog-info files, or things update in external systems, and those changes get streamed in and settle over time inside the catalog. But throwing an error in a processor, instantly aborts processing of that entity and stops its ingestion. Now imagine being a large organization where these changes happen maybe hundreds of times per day. Owners of catalog-info files will constantly be surprised by their files "breaking" in ingestion, maybe a very long time after they were initially created - they were valid at their time of creation and haven't been touched since! This is very frustrating and slows down your users because things break silently for reasons out of your control.

You can throw for "hard" errors such as when the basic expectations of the entity shape are broken, for example, if a [`metadata.annotations` value](./descriptor-format.md#annotations-optional) was an array instead of a string. That does not match the TypeScript contract, and if you accepted such an entity into the catalog, readers of those entities are likely to explode when trying to perform string operations on that value.

## Can I represent versions (of APIs, services etc) in the catalog?

* recommendations
  * versions
    * 👀NOT represent fine grained versions | catalog 👀
      * Reason: 🧠NOT builtin facilities (by design) & the alternatives have significant drawbacks 🧠 
    * ⭐️SOMETIMES, you can represent major, breaking versions -- as -- separate entities ⭐️
      * Reason: 🧠Catalog entities -- generally represent the -- "human concept" of a thing / != technical implementation of it 🧠 
        * plugins -- show -- ALL of the finer details about this "human concept"
* catalog
  * ⚠️scope about the information / displayed | catalog ⚠️
    * _rarely changing_,
    * _human curated_,
    * easily overseen & _managed -- by the -- owners_ of those entities
  * _Examples:_
    * _Example1:_ backend services
      * use case
        * several versions of a service / deployed | several environments | SAME time / change rapidly
    * _Example2:_ software libraries
      * NOT recommended
        * catalog EVERY dependency / EVERY software component 
        * track individual versions of an inner source library -> check a separate solution
      * recommended
        * inner-source libraries / widely used | your org
          * create 1! component 
          * lets people
            * search for it
            * find the owners
            * gain similar insight
    * _Example3:_ APIs 
      * NOT recommended, BUT ONLY solution
        * create entities / EVERY version of your API
          * ⚠️-> confusing ⚠️
