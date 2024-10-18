# storybook

* Storybook build for Backstage
* https://backstage.io/storybook/

## Usage

* `yarn storybook` | repo root
  * run the storybook locally
  * show the default set of stories / published to https://backstage.io/storybook
* if you want to show own stories -> pass >=1 package paths -- as an -- argument
  * _Example:_ show "plugins/search" story 

    ```sh
    yarn storybook plugins/search
    ```

## Why is this not part of `@backstage/core-components`?

* dependency conflicts with `@backstage/cli` 
  * `nohoist`
    * avoid the conflicts
* separated out of `@backstage/core-components`
  * Reason: 🧠you can ONLY use it | private packages 🧠
