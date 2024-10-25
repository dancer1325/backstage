---
id: concepts
title: Search Concepts
description: Documentation on Backstage Search Concepts
---

- [Search Engines](#search-engines)
- [Query Translators](#query-translators)
- [Documents and Indices](#documents-and-indices)
- [Collators](#collators)
- [Decorators](#decorators)
- [The Scheduler](#the-scheduler)
- [The Search Page](#the-search-page)
- [Search Context and Components](#search-context-and-components)

### Search Engines

* Backstage Search
  * != search engine -- _Example:_ Elasticsearch, Lunr, Solr, etc -- 
  * 👁️ == interface, between your Backstage instance -- and a -- Search Engine of your choice 👁️ / 
    * implementation / search engines
  * 👁️comes with in-memory search engine implementation | Lunr 👁️

### Query Translators

* == translation layer / 
  * concrete query language -- abstract query language 
    * Reason: 🧠you can bring ANY search engine / have it's own query language  🧠
  * come with Search Engines

### Documents and Indices

* "Document"
  * := abstract concept / -- can be found by -- searching for it
    * can represent
      * software entity,
      * TechDocs page,
      * ...
  * == metadata fields + title + text + location (_Example:_ URL)
* index
  * == collection of documents / given type

### Collators

* := way to define what can be searched /
  * readable object streams of documents / -- conform a -- minimum set of fields
  * -- responsible for --
    * defining documents of a type
    * collecting documents of a type
* "default" collator
  * provided by some plugins -- _Example:_ Catalog Backend --
  * == factories to start searching across Backstage quickly

### Decorators

* == transform streams / 
  * place between collator (read stream) -- & -- indexer (write stream) | indexing process
  * uses
    * add extra information -- to a -- set of documents | search index 
      * _Example:_ add usage or quality of software entities | Software Catalog
      * -> can be used to search
    * remove metadata,
    * filter out,
    * add extra documents | index-time

### The Scheduler

* TODO:
There are many ways a search index could be built and maintained, but Backstage
Search chooses to completely rebuild indices on a schedule. Different collators
can be configured to refresh at different intervals, depending on how often the
source information is updated. When search indexing is distributed among multiple
backend nodes, coordination to prevent clashes is typically handled by a
distributed `SchedulerServiceTaskRunner`.

### The Search Page

Search pages are very custom things. Not every Backstage instance will want the
same interface! In order to allow you to customize your search experience to
your heart's content, the Search Plugin takes care of state management and other
search logic for you, but most of the layout of a search page lives in a search
page component defined in your Backstage App.

For an example of a simple search page, check
[getting started](./getting-started.md#adding-search-to-the-frontend)

### Search Context and Components

A search experience, like a page, is composed of any number of search
components, which are all wired up using a search context.

Each search experience's context consists of details like a search term,
filters, types, results, and a page cursor for handling pagination. Different
components use this context in different ways. For example, the `<SearchBar />`
can set the search term, `<SearchFilter />` components can set filters, and
search results can be displayed using the `<SearchResult />` component.

The `<SearchResult />` and `<SearchFilter />` components are special, in that
they themselves are extensible. For an example of how to extend these
components, check
[getting started](./getting-started.md#adding-search-to-the-frontend).

If you need even more customization, you can use the search context like any
other React context to create custom search components of your own.
