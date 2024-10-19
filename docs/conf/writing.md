---
id: writing
title: Writing Backstage Configuration Files
description: Documentation on Writing Backstage Configuration Files
---

## File Format

* Configuration
  * stored | `app-config.yaml` files
  * -- is shared between the --
    * frontend
    * backend
  * _Example:_ 

  ```yaml
  app:
    title: Backstage Example App
    baseUrl: http://localhost:3000
  
  backend:
    listen: 0.0.0.0:7007
    baseUrl: http://localhost:7007
  
  organization:
    name: CNCF
  
  proxy:
    /my/api:
      target: https://example.com/api/
      changeOrigin: true
      pathRewrite:
        ^/proxy/my/api/: /
  ```

  * are typically checked in
  * stored | repo / houses rest of the Backstage application
  * particular configuration / is available to each Backstage app -- depends on -- installed
    * plugins
    * packages
  * if you want to view the configuration reference / your own project -> run

    ```sh
    yarn backstage-cli config:docs
    ```

## Environment Variable Overrides

* TODO:
Individual configuration values can be overridden using environment variables
prefixed with `APP_CONFIG_`. Everything following that prefix in the environment
variable name will be used as the config key, with `_` replaced by `.`. For
example, to override the `app.baseUrl` value, set the `APP_CONFIG_app_baseUrl`
environment variable to the desired value.

The value of the environment variable is parsed as JSON, but it will fall back
to being interpreted as a string if it fails to parse. Note that if you for
example want to pass on the string `"false"`, you need to wrap it in double
quotes, e.g. `export APP_CONFIG_example='"false"'`.

While it may be tempting to use environment variable overrides for supplying a
lot of configuration values, we recommend using them sparingly. Try to stick to
using configuration files, and only use environment variables for things like
reusing deployment artifacts across staging and production environments.

Note that environment variables work for frontend configuration too. They are
picked up by the serve tasks of `@backstage/cli` for local development, and are
injected by the entrypoint of the nginx container serving the frontend in a
production build.

## Configuration Files

* \>1, it's possible to have
  * ways
    * bundled
    * remote
  * uses
    * support different environments
    * define configuration / local to specific packages
  * `--config <localRelativePath|url>`
    * way to load several
    * `localRelativePath`
      * -- relative to the -- working directory of the executed process
        * _Example:_ `package/backend`
      * _Example:_ `--config ../../my-config.yaml`
    * `url`
      * _Example:_ `--config https://some.domain.io/app-config.yaml`
      * requirements
        * set the remote option | loadBackendConfig call
    * if NOT specified -> load 
      * `app-config.local.yaml`, if it exists
      * else -> `app-config.yaml` 
    * 👀if it's specified / NOT the default `app-config.yaml` passed -> NOT loaded 👀
    * _Example:_

    ```shell
    yarn start --config ../../app-config.yaml --config ../../app-config.staging.yaml --config https://some.domain.io/app-config.yaml
    ```

* `app-config.local.yaml`
  * add to `.gitignore`
  * uses
    * config
    * secrets for local development

* 👀rules to merge ALL loaded configuration files👀
  * configurations / higher priority -> override configurations / lower priority
  * primitive values -- are completely replaced -- by arrays & their contents
  * objects are merged together deeply
    * == if any of the included configs contain a value / given path -> it will be found
  * `null` value | config file == explicit absence of configuration
    * == reading will NOT fall back to a lower priority config

* 👀priority rules of the configurations👀
  * `APP_CONFIG_` environment variables priority > files priority
  * files / loaded -- via -- `--config` are passed by low -- to -> high priority
    * == last one passed has the highest priority
  * if NO `--config` passed -> `app-config.local.yaml` priority > `app-config.yaml` priority

## Includes and Dynamic Data

Includes are supported via special data loading keys that are prefixed with `$`,
which in turn provide a number of different ways to read in data. To load in an
external configuration value, supply an object with one of the special include
keys, for example `$env` or `$file`. A full list of supported include keys can
be found below. For example, the following will read the config key
`backend.mySecretKey` from the environment variable `MY_SECRET_KEY`:

```yaml
backend:
  mySecretKey:
    $env: MY_SECRET_KEY
```

With the above configuration, calling `config.getString('backend.mySecretKey')`
will return the value of the environment variable `MY_SECRET_KEY` when the
backend started up. All includes are loaded at startup, so changing the contents
of files or environment variables will not be reflected at runtime.

Below is a list of the currently supported methods for loading includes.

### Env Includes

This reads a string value from an environment variable. For example, the
following configuration loads the string value from the `MY_SECRET` environment
variable.

```yaml
$env: MY_SECRET
```

Note however, that it's often more convenient to use
[environment variable substitution](#environment-variable-substitution) instead.

### File Includes

This reads a string value from the entire contents of a text file. The file path
is relative to the source config file. For example, the following reads the
contents of `my-secret.txt` relative to the config file itself:

```yaml
$file: ./my-secret.txt
```

### Including Files

The `$include` keyword can be used to load configuration values from an external
file. It's able to load and parse data from `.json`, `.yml`, and `.yaml` files.
It's also possible to include a URL fragment (`#`) to point to a value at the
given path in the file, using a dot-separated list of keys.

For example, the following would read `my-secret-key` from `my-secrets.json`:

```yaml
$include: ./my-secrets.json#deployment.key
```

Example `my-secrets.json` file:

```json
{
  "deployment": {
    "key": "my-secret-key"
  }
}
```

## Environment Variable Substitution

Configuration files support environment variable substitution via a `${MY_VAR}`
syntax. For example:

```yaml
app:
  baseUrl: https://${HOST}
```

Note that all environment variables must be available, or the entire
configuration value will evaluate to `undefined`.

The substitution syntax can be escaped using `$${...}`, which will be resolved
as `${...}`.

Parameter substitution syntax (e.g. `${MY_VAR:-default-value}`) is also
supported to provide a default or fallback value for a given environment
variable if it is unset, or is declared but has no value. For example:

```yaml
app:
  baseUrl: https://${HOST:-localhost:3000}
```

In the above example, when `HOST` is unset or has no value, it will be
substituted with `localhost:3000`.

## Combining Includes and Environment Variable Substitution

The Includes and Environment Variable Substitutions can be combined to do
something like read a secrets configuration for a specific environment. For
example:

```yaml
integrations:
  github:
    - host: github.com
      apps:
        - $include: secrets.${BACKSTAGE_ENVIRONMENT}.yaml
```

Example `secrets.prod.yaml`:

```yaml
appId: 1
webhookUrl: https://smee.io/foo
clientId: someGithubAppClientId
clientSecret: someGithubAppClientSecret
webhookSecret: someWebhookSecret
privateKey: |
  -----BEGIN RSA PRIVATE KEY-----
  SomeRsaPrivateKeySecurelyStored
  -----END RSA PRIVATE KEY-----
```

**Warning: Sensitive information, such as private keys, should not be hard coded**. We recommend that this entire file should be a secret and stored as such in a secure storage solution like Vault, to ensure they are neither exposed nor misused. This example key part only shows the format on how to use the yaml | syntax to make sure that the key is valid.
