# Actions

Commonly recurring patterns wrapped neatly in re-usable actions.

## Setup

### Yarn (classic)

This setup action is to be used in yarn classic repositories (version < 2). It

- configures the node version from `.nvmrc` which is assumed at the repository root
- grants access to the private registry, using `npm-token` input
- configures caching to restore `node_modules` and `src/node_modules`

It can be used as simple as

```yml
name: Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ExodusMovement/actions/setup/yarn@master
        with:
          npm-token: ${{ secrets.NPM_TOKEN }}
      - run: yarn run deps:install
      - run: yarn lint
      - run: yarn test
```

### Yarn berry

This setup action is to be used in yarn berry repositories (version >= 2).
It differs from the yarn classic setup action in two regards

- it restores `.yarn/cache` instead of `node_modules`
- it grants access to the private registry through `yarn config set` because `.npmrc` files are disregarded by yarn berry

To use it, only the following detail has to be changed compared with the above snippet

```diff
- - uses: ExodusMovement/actions/setup/yarn@master
+ - uses: ExodusMovement/actions/setup/yarn-berry@master
```

### Lerna

This setup action layers on top of the yarn berry action and adds cache retrieval of the nx cache in a lerna monorepo.

Use it as

```yaml
- uses: ExodusMovement/actions/setup/lerna@master
  with:
    npm-token: ${{ secrets.NPM_TOKEN }}
```

or if you need publish permissions

```yaml
- uses: ExodusMovement/actions/setup/lerna@master
  with:
    npm-token: ${{ secrets.NPM_PUBLISH_TOKEN }}
```
