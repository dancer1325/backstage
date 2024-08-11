---
id: index
title: Installing a standalone server
sidebar_label: Introduction
description: How to install Backstage for your own use.
---

Audience: Developers and Admins

## Goal

* Create your very own Backstage customizable app locally / `SQLite` database
  * NOT a production-ready installation


## Prerequisites

* basic knowledge of Linux OS
* GNU-like build environment | CL
  * | Debian/Ubuntu -> `make` and `build-essential` packages installed
  * | MacOS -> XCode command line build tooling -- get it via -- `xcode-select --install`
* `curl` or `wget` installed
* Node.js [Active LTS Release](https://nodejs.org/en/about/previous-releases)
* `yarn` [Installation](https://classic.yarnpkg.com/en/docs/install)
* `docker` [installation](https://docs.docker.com/engine/install/)
* `git` [installation](https://github.com/git-guides/install-git)
* if the system -- is NOT directly accessible over -- your network (_Example:_ | container, VM or remote system) -> open the ports
  * 3000,
  * 7007 

## 1. Create your Backstage App

* Once we set up integrations with your specific data sources -> backstage app demo data is over
* `npx`
  * to install the Backstage Standalone app
  * := tool / 
    * preinstalled with Node.js
    * allows
      * running commands straight from NPM registry or other registries
* TODO:
This command will create a new directory with a Backstage app inside. The wizard will ask you for the name of the app. This name will be created as sub directory in your current working directory.

![create app](../assets/getting-started/create-app-output.png)

Inside that directory, it will generate all the files and folder structure
needed for you to run your app.

### General folder structure

Below is a simplified layout of the files and folders generated when creating an app.

```
app
├── app-config.yaml
├── catalog-info.yaml
├── package.json
└── packages
    ├── app
    └── backend
```

- **app-config.yaml**: Main configuration file for the app. See
  [Configuration](https://backstage.io/docs/conf/) for more information.
- **catalog-info.yaml**: Catalog Entities descriptors. See
  [Descriptor Format of Catalog Entities](https://backstage.io/docs/features/software-catalog/descriptor-format)
  to get started.
- **package.json**: Root package.json for the project. _Note: Be sure that you
  don't add any npm dependencies here as they probably should be installed in
  the intended workspace rather than in the root._
- **packages/**: Lerna leaf packages or "workspaces". Everything here is going
  to be a separate package, managed by lerna.
- **packages/app/**: An fully functioning Backstage frontend app, that acts as a
  good starting point for you to get to know Backstage.
- **packages/backend/**: We include a backend that helps power features such as
  [Authentication](https://backstage.io/docs/auth/),
  [Software Catalog](https://backstage.io/docs/features/software-catalog/),
  [Software Templates](https://backstage.io/docs/features/software-templates/)
  and [TechDocs](https://backstage.io/docs/features/techdocs/)
  amongst other things.

Now, that we know what it does, let's run it!

```bash
npx @backstage/create-app@latest
```

This may take a few minutes to fully install everything. Don't stress if the loading seems to be spinning nonstop, there's a lot going on in the background.

:::note

If this fails on the `yarn install` step, it's likely that you will need to install some additional dependencies which are used to configure `isolated-vm`. You can find out more in their [requirements section](https://github.com/laverdet/isolated-vm#requirements), and then run `yarn install` manually again after you've completed those steps.

:::

## 2. Run the Backstage app

You Backstage app is fully installed and ready to be run! Now that the installation is complete, you can go to the application directory and start the app using the `yarn dev` command. The `yarn dev` command will run both the frontend and backend as separate processes (named `[0]` and `[1]`) in the same window.

```bash
cd my-backstage-app # your app name
yarn dev
```

![Screenshot of the command output, with the message web pack compiled successfully](../assets/getting-started/startup.png)

Here again, there's a small wait for the frontend to start up. Once the frontend is built, your browser window should automatically open.

:::tip Browser window didn't open

When you see the message `[0] webpack compiled successfully`, you can navigate directly to `http://localhost:3000` to see your Backstage app.

:::

You can start exploring the demo immediately.

![Screenshot of the Backstage portal.](../assets/getting-started/portal.png)

## Recap

This tutorial walked through how to deploy Backstage using the `npx @backstage/create-app@latest` command. That command created a new directory that holds your new Backstage app. That app is currently only configured for development purposes, as it is using an in-memory database and contains demo data.

## Next steps

Choose the correct next steps for your user role, if you're likely to be deploying and managing a Backstage instance for your organization, look through the [Admin](#admin) section. If you're likely to be developing on/for Backstage, take a look through the [Developer](#developer) section.

### Admin

- Deploying a production server
  - [Deploying with Docker](../deployment/docker/docker.md)
  - [Deploying with Kubernetes](../deployment/k8s/k8s.md)
  - [Deploying with AWS Lightsail](../deployment/backstage-deploy/aws.md)
- Configuring Backstage
  - [Database](configuring Backstage/Database/database.md)
  - [Authentication](configuring Backstage/authentication.md)
  - [Plugins](configuring Backstage/configure-app-with-plugins.md)
  - [Theme](configuring Backstage/app-custom-theme.md)
  - [Homepage](configuring Backstage/homepage.md)

### Developer

- Using your Backstage instance
  - [Logging into Backstage](using backstage/logging-in.md)
  - [Register a component](using backstage/register-a-component.md)
  - [Create a new component](using backstage/create-a-component.md)

Share your experiences, comments, or suggestions with us:
[on discord](https://discord.gg/backstage-687207715902193673), file issues for any
[feature](https://github.com/backstage/backstage/issues/new?labels=help+wanted&template=feature_template.md)
or
[plugin suggestions](https://github.com/backstage/community-plugins/issues/new/choose),
or
[bugs](https://github.com/backstage/backstage/issues/new?labels=bug&template=bug_template.md)
you have, and feel free to
[contribute](https://github.com/backstage/backstage/blob/master/CONTRIBUTING.md)!
