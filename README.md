# Kardalum Starter

Build with [Kardalum](https://kardalum.com), the free CMS that never loses
your data, never breaks on update, and scales to any size. Every version
of everything you write is kept.

The packages publish under these names in the next release; until then
the commands below print a reservation notice.

This repository is the community home for building **with** Kardalum:
runnable examples, the public issue tracker, and
[Discussions](../../discussions).

## Start a site

```sh
npm create kardalum@latest my-site
```

Then `cd my-site`, `npm run dev`, and open http://localhost:3000/kardalum
to finish setup in your browser (the database choice included). Your
project is plain files you own.

The full guide is at <https://docs.kardalum.com>.

## Run the example site

[`examples/hello-website`](examples/hello-website) is a normal website on
Kardalum's headless API: one plain Node file, no framework, no npm
dependencies. It lists the instance's document types and published
content and renders any item's fields.

```sh
cd examples/hello-website
cp .env.example .env   # then put your instance's origin and API key in .env
node server.js         # open http://localhost:4600
```

Its README also walks through creating a document type and publishing a
"Hello world" page on your own instance.

## Links

- **Docs**: <https://docs.kardalum.com>
- **Marketplace**: <https://marketplace.kardalum.com>
- **Plugin developers**: [kardalum-devkit](https://github.com/kardalum/kardalum-devkit)

## Community

- **Bugs and feature requests**: [Issues](../../issues)
- **Questions, ideas, show and tell**: [Discussions](../../discussions)
- **Plugin toolchain issues**: [kardalum-devkit issues](https://github.com/kardalum/kardalum-devkit/issues)
- **Security reports**: privately, as [SECURITY.md](SECURITY.md) says

Kardalum is free to use. Its packages are on npm as `create-kardalum`,
`kardalum`, `create-kardalum-plugin` and the
[`@kardalum`](https://www.npmjs.com/org/kardalum) scope, licensed
Apache-2.0; see [CONTRIBUTING.md](CONTRIBUTING.md) to take part.
