---
id: overview
title: Kubernetes
sidebar_label: Overview
description: Monitoring Kubernetes based services with the software catalog
---

* Kubernetes | Backstage
  * goal
    * 👁️oriented to service owners (NOT cluster admins) 👁️
  * allows
    * easily checking the health of the services / NO matter how or where those services are deployed (local host, in production, ...)

![Kubernetes plugin screenshot](../../assets/features/kubernetes/backstage-k8s-2-deployments.png)

  * == [`@backstage/plugin-kubernetes`](https://github.com/backstage/backstage/tree/master/plugins/kubernetes) + 
[`@backstage/plugin-kubernetes-backend`](https://github.com/backstage/backstage/tree/master/plugins/kubernetes-backend)
    * `@backstage/plugin-kubernetes`
      * allows
        * exposing easily information to end user 
    * `@backstage/plugin-kubernetes-backend`
      * allows
        * connecting to Kubernetes clusters / collect information 
  * Check
    * [install the Kubernetes plugins](installation.md)
    * [configure them](configuration.md)
