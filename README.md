# Prisma 2 Docs

This repository currently contains the documentation for Prisma 2 which is currently in _Preview_. Once Prisma 2 is released for _General Availability_, the docs will be moved into respective locations on prisma.io.

> Prisma 2 is currently in Preview! [Limitations](https://github.com/prisma/prisma2-docs/blob/master/limitations.md) include missing features, limited performance and stability issues.

## Getting started

The easiest way to get started with [Photon](https://github.com/prisma/photonjs) and/or [Lift](https://github.com/prisma/lift) is by installing the Prisma CLI and running the interactive `init` command:

```
npm install -g prisma2
prisma2 init hello-prisma
```

The interactive prompt will ask you to provide database credentials for your database. If you don't have a database yet, select **SQLite** and let the CLI set up a database file for you.

Learn more about the `prisma2 init` flow [here](./getting-started.md).

## Contents

- [Getting started](./getting-started.md)
- [Tutorial](./tutorial.md)
- [Prisma ecosystem](./prisma-ecosystem.md)
- [Prisma project file](./prisma-project-file.md)
- [Data sources](./data-sources.md)
- [Data modeling](./data-modeling.md)
- [Relations](./relations.md)
- [Prisma 2 CLI](./prisma-2-cli)
- [Introspection](./introspection.md)
- [Limitations](./limitations.md)
- Core
  - Connectors
    - [MySQL](./core/connectors/mysql.md)
    - [PostgreSQL](./core/connectors/postgres.md)
    - [SQLite](./core/connectors/sqlite.md)
    - [MongoDB](./core/connectors/mongo.md)
  - Generators
    - [Photon JS](./core/generators/photon-js.md)
- Photon
  - [API](./photon/api.md)
  - [Use only Photon](./photon/use-only-photon.md)
- Lift
  - [Steps](./lift/steps.md)
  - [Migration files](./lift/migration-files.md)
  - [Use only Lift](./lift/use-only-lift.md)
- [Glossary](./glossary.md)
