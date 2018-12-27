# roosevelt-logger

Intuitive, attractive logger for Node.js applications based on [Winston](https://github.com/winstonjs/winston). This module was built and is maintained by the [Roosevelt web framework team](https://github.com/rooseveltframework/roosevelt), but it can be used independently of Roosevelt as well.

## Install

First declare `roosevelt-logger` as a dependency in your app.

## Usage

Require the package into your application and call its constructor:

```js
const logger = require('roosevelt-logger')()

logger.log('some info')
logger.warn('a warning')
logger.error('an error')
logger.verbose('noisy log only displayed when verbose is enabled')
```

This would output the following:

```
some info
⚠️  a warning
❌  an error
```

## Configure logger

Optionally you can pass the logger a set of configs:

- `info` *[Boolean]*: Enable regular logs.

  - Default: `true`.

- `warnings` *[Boolean]*: Enable logging of warnings.

  - Default: `true`.

- `verbose` *[Boolean]*: Enable verbose (noisy) logging.

  - Default: `false`.

- `disable` *[Array of Strings]*: Disable logging in certain environments. Each entry can be either an environment variable or a set NODE_ENV string.

  - Default: `[]`.
  - Example usage:
    -`['LOADED_MOCHA_OPTS']` (disables logger when being run by [Mocha](https://mochajs.org/).)
    -`['production']` (disables logger when NODE_ENV is set to 'production'.)

- Custom log type *[Object]*: You can also define your own log types and specify what native log type it maps to.

  - API:

    - `enable` *[Boolean]*: Enable this custom log.
      - Default:  `true`.
    - `type` *[String]*: What type of native log this custom log maps to.
      - Default: `info`
      - Allowed values: `info`, `warn`, or `error`.
    - `prefix`: *[String]*: The string that prefixes any log entry. If set to a falsy item (null, an empty string, etc), the prefix will be disabled for the following log type.
      - default: If not set, default to the prefix of the type (i.e.: if the type is `warn`, the prefix will default to `⚠️`)

  - Simple custom type example for a new log type called `dbError`:

    ```json
    {
      "dbError": {}
    }
    ```

  - The above example would create a custom log type `dbError`. Since no params are supplied to it, it defaults to being enabled and defaults to log type `info` and no prefix.

  - Complex custom type example:

    ```json
    {
      "dbError": {
        "enable": false,
        "type": "error",
        "prefix": "🗄"
      }
    }
    ```

## Usage with custom configs

Require the package into your application and call its constructor:

```js
const logger = require('roosevelt-logger')({
    verbose: true,
    dbError: {
        type: "error",
    }
    disable: ['LOADED_MOCHA_OPTS']
})

logger.log('some info')
logger.warn('a warning')
logger.error('an error')
logger.verbose('noisy log only displayed when verbose is enabled')
logger.dbError('custom log for database errors')
```

[TODO: show output of above example.]

## Properties of roosevelt-logger module

In addition to the constructor, roosevelt-logger exposes the following properties:

* `winston`: *[Function]*: The instance of [Winston](https://www.npmjs.com/package/winston) that roosevelt-logger uses internally.
* `winstonLogger`: The specific logger instance of [Winston](https://www.npmjs.com/package/winston) in roosevelt-logger.
* `transports`: The default transports enabled by roosevelt-logger.