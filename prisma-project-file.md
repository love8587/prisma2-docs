# Prisma project file

The Prisma project file (short: project file) is the main configuration file for your Prisma project. It contains the following parts:

- [**Data sources**](./data-sources.md): Specify the details of the data sources Prisma should connect to (e.g. a PostgreSQL database)
- [**Data model definition**](#./data-modeling.md): Specifies your application models (the shape of the data per data source)
- **Generators** (optional): Specifies what data source clients should be generated based on the data model (e.g. Photon JS)

## Naming

The default name for the project file is `project.prisma`. When your project file is named like this, the Prisma 2 CLI will detect it automatically in the directory where you invoke the CLI command.

If the project file is named differently, you can provide an explicit option to the command to point the CLI to the location of the project file.

## Syntax

The project file is written in Prisma Definition Language (PDL). You can find a full reference for PDL in the [spec](https://github.com/prisma/rfcs/blob/0002-datamodel-2/text/0002-datamodel.md).

## Building blocks

### Data sources

A data source can be specified using a `datasource` block in the project file.

#### Fields

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| `provider` | **Yes** | Enum (`postgres`, `mysql`, `sqlite`) | Describes which data source connector to use. |
| `url` | **Yes** | String (URL) | Connection URL including authentication info. Each data source connector documents the URL syntax. Most connectors use the syntax provided by the database. |
| `enabled` | No | Boolean | Use environment variables to enable/disable a data source. **Default**: `false`. |

A data source connector may bring its own fields to allow users to tailor their data models according to specific features of the connected data sources.

#### Examples

```groovy
datasource pg {
  provider = "postgres"
  url      = env(POSTGRES_URL)
  enabled  = true
}

datasource mgo {
  provider = "mongodb"
  url      = env(MONGO_URL)
}

datasource mgo2 {
  provider = "mongodb"
  url      = env(MONGO2_URL)
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
  target   = "es3"
  provider = "photon-js"
  output   = "./client"
}

generator ts {
  target   = "es5"
  provider = "./path/to/custom/generator"
}

generator go {
  snakeCase = true
  provider  = "go"
}
```

### Data model definition
