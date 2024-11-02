---
id: techdocs-overview
title: TechDocs Documentation
sidebar_label: Overview
# prettier-ignore
description: TechDocs is Spotify’s homegrown docs-like-code solution built directly into Backstage
---

## What is it?

<!-- Intro, backstory, etc.: -->

* TechDocs
  * == docs-like-code solution
    * write documentation | Markdown files / live close with the code
  * 👀core products | Spotify’s developer experience 👀
    * 5000+ documentation sites
    * 10000 average daily hits
  * [announcement blog post](https://backstage.io/blog/2020/09/08/announcing-tech-docs)🎉

## Features

- Deploy TechDocs / NO matter set up of software environment 
- Create documentation-only sites
- Take advantage of
  - [TechDocs Addon Framework](addons.md)
    - to add features | base docs-like-code experience.
  - [MkDocs plugins](https://www.mkdocs.org/user-guide/plugins/)
    - to create a rich reading experience
- Search for and find docs | Service's page in Backstage Catalog

## Project roadmap

### Now

No current plans.

### Next

No current plans.

### Someday/Maybe

* TODO:
- What can we do in TechDocs to help drive up documentation quality? We have many ideas, for example, a Trust Card with associated Trust Score and automatic triggering of documentation maintenance notifications.
- Contribute to and deploy from a marketplace of TechDocs Addons
- Addon: MDX (allows you to use JSX in your Markdown content)
- Can we go static site generator agnostic?
- Better integration with
  [Scaffolder V2](https://github.com/backstage/backstage/issues/2771) (e.g. easy to choose and apply documentation template with Software Templates)
- Possible to configure several aspects about TechDocs (e.g. URL, homepage,
  theme)

### Done

See [Done](#done)

## Supported

### Source code hosting providers

| Source Code Hosting Provider | Support Status |
| ---------------------------- | -------------- |
| GitHub                       | Yes ✅         |
| GitHub Enterprise            | Yes ✅         |
| Bitbucket                    | Yes ✅         |
| Azure DevOps                 | Yes ✅         |
| Gerrit                       | Yes ✅         |
| GitLab                       | Yes ✅         |
| GitLab Enterprise            | Yes ✅         |
| Gitea                        | Yes ✅         |
| AWS CodeCommit               | Yes ✅         |
| Harness Code                 | Yes ✅         |

### File storage providers

| File Storage Provider             | Support Status |
| --------------------------------- | -------------- |
| Local Filesystem of Backstage app | Yes ✅         |
| Google Cloud Storage (GCS)        | Yes ✅         |
| Amazon Web Services (AWS) S3      | Yes ✅         |
| Azure Blob Storage                | Yes ✅         |
| OpenStack Swift                   | Community ✅   |

[Reach out to us](#get-involved) if you want to request more providers.

## Tech stack

| Stack                                           | Location                                                      |
| ----------------------------------------------- | ------------------------------------------------------------- |
| Frontend Plugin                                 | [@backstage/plugin-techdocs][techdocs/frontend]               |
| Frontend Plugin Library                         | [@backstage/plugin-techdocs-react][techdocs/frontend-library] |
| Backend Plugin                                  | [@backstage/plugin-techdocs-backend][techdocs/backend]        |
| CLI (for local development and generating docs) | [@techdocs/cli][techdocs/cli]                                 |
| Docker Container (for generating docs)          | [techdocs-container][techdocs/container]                      |

[techdocs/frontend]: https://github.com/backstage/backstage/blob/master/plugins/techdocs
[techdocs/frontend-library]: https://github.com/backstage/backstage/blob/master/plugins/techdocs-react
[techdocs/backend]: https://github.com/backstage/backstage/blob/master/plugins/techdocs-backend
[techdocs/container]: https://github.com/backstage/techdocs-container
[techdocs/cli]: https://github.com/backstage/backstage/blob/master/packages/techdocs-cli

## Get involved

Reach out to us in the **#techdocs** channel of our
[Discord chatroom](https://github.com/backstage/backstage#community).

## Done

**Alpha release**

[Milestone](https://github.com/backstage/backstage/milestone/16)

- Alpha of TechDocs that you can use end to end - and contribute to.

**Beta release**

[Milestone](https://github.com/backstage/backstage/milestone/29)

- TechDocs' recommended setup supports most environments (CI systems, cloud
  storage solutions, source control systems).
- [Instructions for upgrading from Alpha to Beta](how-to-guides.md#how-to-migrate-from-techdocs-alpha-to-beta)

**v1.0**

TechDocs promoted to v1.0! To understand how this change affects the package, check out our [versioning policy](https://backstage.io/docs/overview/versioning-policy).

TechDocs packages:

- @backstage/plugin-techdocs
- @backstage/plugin-techdocs-backend
- @backstage/plugin-techdocs-node
- @techdocs/cli

**TechDocs Addon Framework**

With the Backstage 1.2 release, we introduced the [TechDocs Addon Framework](https://backstage.io/blog/2022/05/13/techdocs-addon-framework) for augmenting the TechDocs experience at read-time.

In addition to the framework itself, we open sourced a **ReportIssue** Addon, helping you to create a feedback loop that drives up documentation quality and foster a documentation culture at your organization.
