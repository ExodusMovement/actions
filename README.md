# Actions

Commonly recurring patterns wrapped neatly in re-usable actions.

## Setup

### Yarn (classic)

This setup action is to be used in yarn classic repositories (version < 2). It

- configures the node version from `.nvmrc` which is assumed at the repository root
- grants access to the private registry, using the organization secret `NPM_TOKEN`
- configures caching to restore `node_modules` and `src/node_modules`

It can be used as simple as:

```yml
name: Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: ExodusMovement/actions/setup/yarn@v1
      - run: yarn run deps:install
      - run: yarn lint
      - run: yarn test
```

### Yarn berry

This setup action is to be used in yarn berry repositories (version >= 2).
It differs from the yarn classic setup action in two regards

- it restores `.yarn/cache` instead of `node_modules`
- it grants access to the private registry through `yarn config set` because `.npmrc` files are disregarded by yarn berry

To use it, only the following detail has to be changed compared with the above snippet:

```diff
- - uses: ExodusMovement/actions/setup/yarn@v1
+ - uses: ExodusMovement/actions/setup/yarn-berry@v1
```

### Lerna

This setup action layers on top of the yarn berry action and adds cache retrieval of the nx cache in a lerna monorepo.

Use it as:

```yaml
- uses: ExodusMovement/actions/setup/lerna@v1
```
