* Goal
  * create your own Backstage customizable app /
    * run locally
    * 'SQlite' database

## Prerequisites
* OS Unix-based
* GNU-like build environment
  * Debian / Ubuntu
    * make
    * build-essential
  * MacOs
    * xcode-select --install
      * `xcode-select --version` check that it's installed
* `curl` or `wget` packages installed
* [Node.js](https://nodejs.org/en/about/previous-releases)
* [yarn](https://classic.yarnpkg.com/en/) 
* [docker](https://www.docker.com/)
* [git](https://www.git-scm.com/)
* ports available
  * 3000
  * 7007

## Steps
* `npx @backstage/create-app@latest`
  * Check under '/examples/my-backstage-app'
  * [Folder structure](https://backstage.io/docs/getting-started/#general-folder-structure)
* `yarn install` & `yarn dev`
  * run FE & BE / separated processes | same window
  * Opened in your browser automatically or go to 'localhost:3000'
