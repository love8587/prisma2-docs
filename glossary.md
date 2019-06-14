# Glossary

### Composite model

A composite model is a model that doesn't directly map to a structure (e.g. a _table_ or a _collection_) in the underlying data source. Instead, it's composed out of multiple parts from the underlying database.

### Data source

A data source can be anything that Prisma can connect to via a [connector](#data-source-connector), e.g. a database, a GraphQL API, a REST API or a 3rd-party service.

### Data source connector

Also sometimes referred to as:

- Connector

A data source connector connects Prisma to a [data source](#data-source).

### Prisma Definition Language (PDL)

PDL is the name of the syntax used to write a [project file](#prisma-project-file).

> Learn more about PDL in the [spec](https://github.com/prisma/rfcs/blob/0002-datamodel-2/text/0002-datamodel.md).

### Embed

Also sometimes referred to as:

- Embedded type
- Embedded model
- Embedded structure

### Generator

A generator determines what kind of code should be generated from the [data model](#data-model-definition). For example, you can specify the _Photon JS generator_ to generate Photon JS as a type-safe database client based on the data model.

You can include various generators in your [project file](#prisma-project-file). When running `prisma2 generate`, the Prisma CLI reads the specified generators from the project definition and invokes each of them.

### Lift

A declarative data modeling and migration system. Learn more on the [Lift website](https://lift.prisma.io/).

### Migration

Also sometimes referred to as:

- Database migration
- Schema migration

A migration is a concept from [Lift](). It refers to the transition from a data model state to another data model state. 

### Migration engine

The migration engine generates the database operations needed to apply a migration to a database.

### Model

Models represent the entities of your application domain. Non-[composite](#composite-model) models also directly map to structures in the underlying data source, e.g. a _table_ for a relational database or a _collection_ for a document database. The generated Photon API will expose CRUD operations for each model in your [data model](#data-model-definition).

### Data model definition

Also sometimes referred to as: 

- Data model (or datamodel)
- Prisma schema
- Application schema
- Model schema

Contains the definitions of all your models. The data model definition is part of the [project file](#prisma-project-file).

### Nested write

Photon lets you perform nested creates and nested updates for related models. A nested write is always performed as an atomic transaction. 

### Photon

An auto-generated type-safe database client. Some people call it an ORM. 

### Prisma project file

Also sometimes referred to as:

- Project file

The Prisma project file specifies the main parts of your Prisma project:

- [**Connectors**](#data-source-connector): Specify which data sources Prisma should connect to (e.g. a PostgreSQL database)
- [**Data model definition**](#data-model-definition): Specifies the shape of the data per data source
- [**Generators**](#generator): Specifies what application code should be generated (e.g. Photon JS)

### Selection set

Determins what fields of a model are returned in a Photon API call. By default, the selection set contains all (non-lazy) scalar fields of a model. The selection set can be manipulated by passing the `select` or `include` option to a Photon API call.

### Query engine

The query engine generates and optimizes database queries based on incoming requests from Photon. 