---
id: architecture
title: Search Architecture
description: Documentation on Search Architecture
---

* goals of the designed architecture
  * support a wide variety of search engines /
    * 1! developer experience
    * good out-of-the-box experience
  * scheduled, bulk index management
  * search across the ENTIRE Backstage 
    * == -- NOT limited to -- entities | software catalog
  * deploy Backstage -- via -- ANY search engine
    * how?
      * core search plugin -- must have integration and translation layer with -- search engine
    * backend API endpoint -- can be replaced with a -- custom endpoint

![Search Architecture](../../assets/search/architecture.drawio.svg)

* NON-goals of the designed architecture
  * event-driven or incremental index management

* allows
  * any plugin exposes NEW content to search
  * any plugin -- can append -- relevant metadata | existing content in search
  * refine search queries (e.g. ranking, scoring, etc.)
  * customize the search UI
  * add search functionality | any Backstage plugin or deployment

## Tech Stack

| Stack                     | Location                                              |
| ------------------------- | ----------------------------------------------------- |
| Frontend Plugin           | @backstage/plugin-search                              |
| Frontend Plugin Library   | @backstage/plugin-search-react                        |
| Isomorphic Plugin Library | @backstage/plugin-search-common                       |
| Backend Plugin            | @backstage/plugin-search-backend                      |
| Backend Plugin Library    | @backstage/plugin-search-backend-node                 |
| Backend Plugin Module     | @backstage/plugin-search-backend-module-elasticsearch |
| Backend Plugin Module     | @backstage/plugin-search-backend-module-pg            |
