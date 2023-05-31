# primer-changesets-cli

> The Primer flavored changesets CLI

This CLI is a think wrapper around [the `changesets` CLI](https://github.com/changesets/changesets/tree/main/packages/cli) which adds one additional question:

> Which modules have been changed?

Depending on in which repository the CLI is run, it will either suggest a module from [primer/react](https://github.com/primer/react) or [primer/view_components](https://github.com/primer/view_components).

Run with

```
npx primer-changesets-cli@latest
```

## How it works

Unfortunately the the `@changeset/cli` package is not decomposable, we cannot easily import selected modules from it, inject our additional question, and then compose it together again before running it.

So instead we use [`patch-package`](https://github.com/ds300/patch-package) to patch the `@changeset/cli` package directly in a maintainable way. The changed lines can be seen in the [`patch/` folder](patch/). They get applied using the `postinstall` script in this package when installing locally.

In order for it to work when running the command with `npx primer-changesets-cli` later, we copy the patched file from `node_modules/@changesets/cli/dist/cli.cjs.dev.js` into the local `bin.js` directly, using the `prepublishOnly` script.

## License

[MIT](LICENSE)
