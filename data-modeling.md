# Data modeling

## Data model definition

The data model definition (short: data model or datamodel) is part of your [project file](./prisma-project-file.md).

It describes the shape of the data per data source. For example, when connecting to a _relational database_ as a data source, the data model definition is a declarative representation of the _database schema_ (tables, columns, indexes, ...). For a REST API, it describes the shapes of the _resources_ that can be retrieved and manipulated via the API.

## Building blocks

### Models

Models represent the entities of your application domain. They are defined using `model` blocks in the data model.

On a technical level, a model maps to the underlying structures of the data source, for example:

- In PostgreSQL, a model maps to a _table_
- In MongoDB, a model maps to a _collection_
- In REST, a model maps to a _resource_

#### Examples

```groovy
model User {
  id         Int       @id
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt
  email      String    @unique
  posts      Post[]
}

model Post {
  id          Int        @id
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  title       String
  draft       Boolean
  categories  String[]
  slug        String
  author      User
  comments    Comment[]

  @@unique([ title, slug ])
}

model Comment {
  id         Int       @id
  createdAt  DateTime  @default(now())
  updatedAt  DateTime  @updatedAt
  email      String?
  comment    String
}
```

#### Naming models

Models are typically spelled in [PascalCase](http://wiki.c2.com/?PascalCase) and use the _singular_ form (e.g. `User` instead of `Users`).

Technically, a model can be named anything that adheres to this regular expressions: 

```
[A-Za-z_][A-Za-z0-9_]*
```

#### Model operations in the Photon API (CRUD)

Every _model_ in the data model definition will result into a number of CRUD operations:

- `findMany`
- `findOne`
- `create`
- `update`
- `upsert`
- `delete`
- `updateMany`
- `deleteMany`

The operations are accessible via a generated property on the Photon instance. By default the name of the property is the plural, lowercase form of the model name, e.g. `users` for a `User` model or `posts` for a `Post` model. You can configure the name of the property in the data model.

Here is an example illustrating the use of a `users` property from the Photon JS API:

```js
const newUser = await photon.users.create({ data: {
  name: "Alice"
}})
const allUsers = await photon.users.findMany()
```

### Fields

The properties of a [model](#models) are called _fields_. A field consists of several parts:

- [Name](#naming-fields)
- [Type](#types)
- [Type modifier](#type-modifiers) (optional)
- [Attributes](#attributes) (optional)

You can see examples of fields on the sample models [above](#examples).

#### Naming fields

Field names are typically spelled in [camelCase](http://wiki.c2.com/?CamelCase) starting with a lowercase letter.

Technically, a model can be named anything that adheres to this regular expressions: 

```
[A-Za-z_][A-Za-z0-9_]*
```

#### Types

The type of a field determines its _structure_. A type falls in either of three categories:

- [Scalar type](#scalar-types) (includes [enums](#enums))
- [Model](#models)
- [Embed](#embeds)

#### Type modifiers

#### Attributes

#### 

### Embeds

### Enums

### Type definitions

### Attributes

#### Field-level attributes

#### Model-level attributes

### Functions

### Scalar types