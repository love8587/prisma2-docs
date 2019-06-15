# Data modeling

## Data model definition

The data model definition (short: data model or datamodel) is part of your [project file](./prisma-project-file.md).

It describes the shape of the data per data source. For example, when connecting to a _relational database_ as a data source, the data model definition is a declarative representation of the _database schema_ (tables, columns, indexes, ...). For a REST API, it describes the shapes of the _resources_ that can be retrieved and manipulated via the API.

## Example

Here is an example based on a local SQLite database located in the same directory of the project file (called `data.db`):

```groovy
datasource mysql {
  url      = "file:data.db"
  provider = "sqlite"
}

model User {
  id        Int      @id
  createdAt DateTime @default(now())
  email     String   @unique
  name      String?
  role      Role     @default(USER)
  posts     Post[]
  profile   Profile?
  address   Address?
}

embed Address {
  street: String
  zipCode: String
}

model Profile {
  id   Int    @id
  user User
  bio  String
}

model Post {
  id         Int        @id
  createdAt  DateTime   @default(now())
  updatedAt  DateTime   @updatedAt
  author     User
  title      String
  published  Boolean    @default(false)
  categories Category[]
}

model Category {
  id    Int    @id
  name  String
  posts Post[]
}

enum Role {
  USER
  ADMIN
}
```

## Building blocks

### Models

Models represent the entities of your application domain. They are defined using `model` blocks in the data model.

On a technical level, a model maps to the underlying structures of the data source, for example:

- In PostgreSQL, a model maps to a _table_
- In MongoDB, a model maps to a _collection_
- In REST, a model maps to a _resource_

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

Note that for Photon JS the name of the `users` property is auto-generated using the [`pluralize`](https://github.com/blakeembrey/pluralize) package. 

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

The type of a field can be modified by appending either of two modifiers:

- `[]`: Make a field a **list** 
- `?`: Make a field **optional**

In the main example above, the field `name` on the `User` model is _optional_ and the `posts` field is a _list_.

Lists can also be optional and will give the list a third state (which is `null`):

- `Blog[]`: Empty list or non-empty list (default: `[]`)
- `Blog[]?`: `null`, empty list or non-empty list (default: `null`)

The default value for a required list is an empty list. The default value for an optional list is `null`.

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

### Relations