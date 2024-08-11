---
id: search-overview
title: Search Documentation
sidebar_label: Overview
# prettier-ignore
description: Backstage Search lets you find the right information you are looking for in the Backstage ecosystem.
---

# Backstage Search

* allows
  * finding information | Backstage ecosystem
  * extending it -- via -- creating collators  
  * creating composable search page experiences
  * customizing the search result
  * bringing your own search engine
* Check the 
  * [architecture](./architecture.md)
  * [tech stack](./architecture.md#tech-stack)
* Roadmap
  * Check [Backstage issues labeled `search`](https://github.com/backstage/backstage/issues?q=is%3Aopen+is%3Aissue+label%3Asearch)
* Plugins / -- integrated with -- Backstage Search

| Plugin                                                                                                                                                                        | Support Status |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| Software Catalog                                                                                                                                                              | ✅             |
| [TechDocs](./how-to-guides.md#how-to-index-techdocs-documents)                                                                                                                | ✅             |
| [Stack Overflow](https://github.com/backstage/backstage/blob/master/plugins/search-backend-module-stack-overflow-collator/README.md#index-stack-overflow-questions-to-search) | ✅             |

* Search engines supported

| Search Engines                                                | Support Status |
| ------------------------------------------------------------- | -------------- |
| [Elasticsearch/OpenSearch](./search-engines.md#elasticsearch) | ✅             |
| [Lunr](./search-engines.md#lunr)                              | ✅             |
| [Postgres](./search-engines.md#postgres)                      | Community ✅   |
