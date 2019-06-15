# Prisma project file

The Prisma project file (short: project file) is the main configuration file for your Prisma project. It contains the following parts:

- [**Data sources**](./data-sources.md): Specify the details of the data sources Prisma should connect to (e.g. a PostgreSQL database)
- [**Data model definition**](#./data-modeling.md): Specifies your application models (the shape of the data per data source)
- **Generators** (optional): Specifies what data source clients should be generated based on the data model (e.g. Photon JS)

Whenever a `prisma2` command is invoked, the CLI typically reads some information from the project file, e.g.:

- `prisma2 generate`: Reads _all_ above mentioned information from the datamodel to generated the right data source client code (e.g. Photon JS).
- `prisma2 lift save`: Reads the data sources and data model definition to create a new [migration](). 

You can also [use environment variables]() inside the project file to provide configuration options when a CLI command is invoked. 

## Naming

The default name for the project file is `project.prisma`. When your project file is named like this, the Prisma 2 CLI will detect it automatically in the directory where you invoke the CLI command.

If the project file is named differently, you can provide an explicit option to the command to point the CLI to the location of the project file.

## Syntax

The project file is written in Prisma Definition Language (PDL). You can find a full reference for PDL in the [spec](https://github.com/prisma/rfcs/blob/0002-datamodel-2/text/0002-datamodel.md).

## Using environment variables

You can use environment variables to provide configuration options when a CLI command is invoked. This is helpful e.g. to:

- Keep secrets out of the project file
- Improve portability of the project file

### The `env` function

Environment variables can be provided using the `env` function:

```
datasource pg {
  provider = "postgres"
  url      = env("POSTGRES_URL")
}
```

When a [`generator`]() block is specified in the project file, the generated code will reference the same environment variables. For example for Photon JS, the generated code could include:

```
childProcess.spawn('./query_engine', {
  env: {
    url: process.env.POSTGRES_URL,
  },
})
```

### Switching data sources based on environments

Sometimes it's helpful to target different environments based in the same project file, for example:

```groovy
datasource db {
  enabled   = env("SQLITE_URL")
  provider  = "sqlite"
  url       = env("SQLITE_URL")
}

datasource db {
  enabled   = env("POSTGRES_URL")
  provider  = "postgresql"
  url       = env("POSTGRES_URL")
}

model User {
  id         Int    @id @db.int
  first_name String @unique
}
```

Depending on which environment variable is set (in this case `SQLITE_URL` or `POSTGRES_URL`), the respective data source will be used.

## Building blocks

### Data sources

A data source can be specified using a `datasource` block in the project file.

#### Fields

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `provider` | **Yes** | Enum (`postgres`, `mysql`, `sqlite`) | Describes which data source connector to use. |
| `url` | **Yes** | String (URL) | Connection URL including authentication info. Each data source connector documents the URL syntax. Most connectors use the syntax provided by the database. |
| `enabled` | No | Boolean | Use environment variables to enable/disable a data source. **Default**: `true`. |

A data source connector may bring its own fields to allow users to tailor their data models according to specific features of the connected data sources.

#### Naming conventions

Data sources are typically named according to the `provider`:

```groovy
datasource sqlite {
  provider  = "sqlite"
  url       = env("SQLITE_URL")
}

datasource mysql {
  provider  = "mysql"
  url       = env("SQLITE_URL")
}

datasource postgres {
  provider  = "postgres"
  url       = env("SQLITE_URL")
}

datasource mongo {
  provider  = "mongo"
  url       = env("SQLITE_URL")
}
```

This is just a general convention, technically data sources can be named anything. Lowercase spelling is typically preferred. There might be special circumstances, such as [switching data sources based on environments](#switching-data-sources-based-on-environments), when it can make sense to apply a different naming scheme. It's also fine to abbreviate a data source name where it doesn't obscure the naming, e.g. `pg` for `postgres` works, but `msql` for `mysql` does not.


#### Examples

```groovy
datasource pg {
  provider = "postgres"
  url      = env(POSTGRES_URL)
  enabled  = true
}

datasource mysql {
  provider = "mongodb"
  url      = env(MYSQL_URL)
}

datasource mongo {
  provider = "mongodb"
  url      = env(MONGO_URL)
}
```

### Generators (optional)

A generator configures what data source clients are generated and how they're generated. Language preferences and configuration will go in here.

#### Fields

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `provider` | **Yes** | String (file path) or Enum (`javascript`, `typescript`, `golang`) | Describes which generator to use. This can point to a file that implements a generator or specify a built-in generator directly. |
| `output` | **Yes** | String (file path) | Determines the location for the generated client. |

A generator may bring its own fields to allow users to customize the generation behaviour.

#### Examples

```groovy
generator js {
  provider = "photon-js"
  output   = "./generator/photon"
}

generator ts {
  provider = "./path/to/custom/generator"
}

generator go {
  snakeCase = true
  provider  = "go"
}
```

### Data model definition
