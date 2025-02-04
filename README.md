# Action: Hugo Link Check 

This action will check for broken links in a Hugo generated static webpage.

See a running example [here](https://github.com/BoundfoxStudios/community-project/blob/develop/.github/workflows/documentation.yml).

## Usage

### Inputs

All inputs are optional.

| Input                    | Description                                                                                    | Default         |
|--------------------------|------------------------------------------------------------------------------------------------|-----------------|
| `fail-on-broken-links`   | The number of required broken links to fail the action. Set to 0 to deactivate                 | `1`             |
| `log-skipped-links`      | Logs skipped links and sends them to skipped-links output                                      | `false`         |
| `retry`                  | Automatically retry requests that return HTTP 429 responses and include a 'retry-after' header | `true`          |
| `timeout`                | Request timeout in ms. Set to 0 for no timeout                                                 | `5000`          |
| `skip`                   | List of urls in regexy form to not include in the check                                        | ``              |
| `hugo-root`              | Base path to your hugo project                                                                 | `./`            |
| `hugo-content-dir`       | Base path to your hugo content directory                                                       | `./content`     |
| `hugo-config`            | Base path to your hugo config                                                                  | `./config.yaml` |
| `hugo-startup-wait-time` | Maximum time to wait for hugo to start up and process your project                             | `20`            |

### Outputs

| Output                | Description                   |
|-----------------------|-------------------------------|
| `broken-links-count`  | Count of broken links         |
| `broken-links`        | JSON-Array with broken links  |
| `skipped-links-count` | Count of skipped links        | 
| `skipped-links`       | JSON-Array with skipped links |

### Mandatory: package.json

To easily support multiple versions of Hugo, you need to have a `package.json` in your `hugo-root`.
In that `package.json` you need a `devDependency` to `hugo-bin` specifying the hugo version to use for your project.
See an example [here](https://github.com/BoundfoxStudios/fairy-tale-defender/blob/develop/docs/package.json).

Minimal example:

```json
{
  "name": "hugo",
  "private": true,
  "hugo-bin": {
    "buildTags": "extended"
  },
  "devDependencies": {
    "hugo-bin": "0.92.3"
  }
}
```

> If you have another idea how to easily support multiple hugo version, please let me know!

### Example

```yaml
name: Example

on: [ push ]

jobs:
  check-broken-links:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: BoundfoxStudios/action-hugo-link-check@v1
        with:
          hugo-root: docs
          hugo-content-dir: docs/content
          hugo-config: docs/config.yaml
```

## Remarks

### Upgrade from v1 to v2

If you're upgrading from v1 to v2 the underlying link check library changed from [broken-link-checker](https://www.npmjs.com/package/broken-link-checker) to [linkinator](https://www.npmjs.com/package/linkinator).
With that, all available non hugo-based options have changed from v1 to v2. 
Please consult the docs for the current options to use with this action.
