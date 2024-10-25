> [!NOTE]
> 🏖 From Monday October 28th through November 4th, maintainers and Spotify employees will be on vacation due to Wellness Week. Expect the project to move a little slower than normal, and support to be limited. Normal service will resume after that! 🏝

[![headline](docs/assets/headline.png)](https://backstage.io/)

# [Backstage](https://backstage.io)

English \| [한국어](README-ko_kr.md) \| [中文版](README-zh_Hans.md) \| [Français](README-fr_FR.md)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![CNCF Status](https://img.shields.io/badge/cncf%20status-incubation-blue.svg)](https://www.cncf.io/projects)
[![Discord](https://img.shields.io/discord/687207715902193673?logo=discord&label=Discord&color=5865F2&logoColor=white)](https://discord.gg/backstage-687207715902193673)
![Code style](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)
[![Codecov](https://img.shields.io/codecov/c/github/backstage/backstage)](https://codecov.io/gh/backstage/backstage)
[![](https://img.shields.io/github/v/release/backstage/backstage)](https://github.com/backstage/backstage/releases)
[![OpenSSF Best Practices](https://bestpractices.coreinfrastructure.org/projects/7678/badge)](https://bestpractices.coreinfrastructure.org/projects/7678)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/backstage/backstage/badge)](https://securityscorecards.dev/viewer/?uri=github.com/backstage/backstage)

## What is Backstage?

* := Framework for building developer portals / 
  * open source
    * originally created by Spotify
    * now hosted by the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io)
  * == centralized software catalog
    * microservices
    * infrastructure tooling
    * documentation
  * allows
    * shipping high-quality code quickly / autonomy NOT compromised

![software-catalog](docs/assets/header.png)

### Features
- [Backstage Software Catalog](https://backstage.io/docs/features/software-catalog/)
  - for managing all your software
    - _Example:_ microservices, libraries, data pipelines, websites, ML models, PR status, resource monitoring, testing, ...
- [Backstage Software Templates](https://backstage.io/docs/features/software-templates/) 
  - for
    - quickly spinning up new projects / CI already configured
    - standardizing your tooling -- via -- your organization’s best practices (language program, CI tool, Cloud provider, ...)
- [Backstage TechDocs](https://backstage.io/docs/features/techdocs/) 
  - for making it easy about technical documentation -- via -- "docs like code" approach,
    - to
      - create
      - maintain
      - find
      - use
    - in Markdown files -- alongside the -- code
- Growing ecosystem of [open source plugins](https://github.com/backstage/backstage/tree/master/plugins) / expand Backstage’s customizability and functionality
  - you can create your own ones
- [Backstage Search](https://backstage.io/docs/features/search/)
  - allows
    - choosing search engine
    - finding all easier
- [Backstage Kubernetes](https://backstage.io/docs/features/kubernetes/)
  - -- done for -- service owners (NOT cluster admins)
  - allows
    - monitoring easily all your services containerized
    - freedom to choose any Kubernetes service (cloud-related)
    - 1! UI / any stage

## Project roadmap

For information about the detailed project roadmap including delivered milestones, see [the Roadmap](https://backstage.io/docs/overview/roadmap).

## Getting Started

To start using Backstage, see the [Getting Started documentation](https://backstage.io/docs/getting-started).

## Documentation

The documentation of Backstage includes:

- [Main documentation](https://backstage.io/docs)
- [Software Catalog](https://backstage.io/docs/features/software-catalog/)
- [Architecture](https://backstage.io/docs/overview/architecture-overview) ([Decisions](https://backstage.io/docs/architecture-decisions/))
- [Designing for Backstage](https://backstage.io/docs/dls/design)
- [Storybook - UI components](https://backstage.io/storybook)

## Community

To engage with our community, you can use the following resources:

- [Discord chatroom](https://discord.gg/backstage-687207715902193673) - Get support or discuss the project
- [Contributing to Backstage](https://github.com/backstage/backstage/blob/master/CONTRIBUTING.md) - Start here if you want to contribute
- [RFCs](https://github.com/backstage/backstage/labels/rfc) - Help shape the technical direction
- [FAQ](https://backstage.io/docs/faq) - Frequently Asked Questions
- [Code of Conduct](CODE_OF_CONDUCT.md) - This is how we roll
- [Adopters](ADOPTERS.md) - Companies already using Backstage
- [Blog](https://backstage.io/blog/) - Announcements and updates
- [Newsletter](https://spoti.fi/backstagenewsletter) - Subscribe to our email newsletter
- [Backstage Community Sessions](https://github.com/backstage/community) - Join monthly meetups and explore Backstage community
- Give us a star ⭐️ - If you are using Backstage or think it is an interesting project, we would love a star ❤️

## Governance

See the [GOVERNANCE.md](https://github.com/backstage/community/blob/main/GOVERNANCE.md) document in the [backstage/community](https://github.com/backstage/community) repository.

## License

Copyright 2020-2024 © The Backstage Authors. All rights reserved. The Linux Foundation has registered trademarks and uses trademarks. For a list of trademarks of The Linux Foundation, please see our Trademark Usage page: https://www.linuxfoundation.org/trademark-usage

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0

## Security

Please report sensitive security issues using Spotify's [bug-bounty program](https://hackerone.com/spotify) rather than GitHub.

For further details, see our complete [security release process](SECURITY.md).
