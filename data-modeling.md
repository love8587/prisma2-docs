# Data modeling

- [Data model definition](#data-model-definition)
- [Example](#example)
- [Building blocks](#building-blocks)
  - [Models](#models)
  - [Fields](#fields)
  - [Embeds](#embeds)
  - [Enums](#enums)
  - [Type definitions](#type-definitions)
  - [Attributes](#attributes)
  - [Functions](#functions)
  - [Scalar types](#scalar-types)
  - [Relations](#relations)

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

## Models

Models represent the entities of your application domain. They are defined using `model` blocks in the data model.

On a technical level, a model maps to the underlying structures of the data source, for example:

- In PostgreSQL, a model maps to a _table_
- In MongoDB, a model maps to a _collection_
- In REST, a model maps to a _resource_

### Naming models

Models are typically spelled in [PascalCase](http://wiki.c2.com/?PascalCase) and use the _singular_ form (e.g. `User` instead of `Users`).

Technically, a model can be named anything that adheres to this regular expression: 

```
[A-Za-z_][A-Za-z0-9_]*
```

### Model operations in the Photon API (CRUD)

Every _model_ in the data model definition will result into a number of CRUD operations in the generated [Photon API]():

- `findMany`
- `findOne`
- `create`
- `update`
- `upsert`
- `delete`
- `updateMany`
- `deleteMany`

The operations are accessible via a generated property on the Photon instance. By default the name of the property is the plural, lowercase form of the model name, e.g. `users` for a `User` model or `posts` for a `Post` model. 

Here is an example illustrating the use of a `users` property from the [Photon JS API](./photon/api.md):

```js
const newUser = await photon.users.create({ data: {
  name: "Alice"
}})
const allUsers = await photon.users.findMany()
```

Note that for Photon JS the name of the `users` property is auto-generated using the [`pluralize`](https://github.com/blakeembrey/pluralize) package. 

## Fields

The properties of a [model](#models) are called _fields_. A field consists of several parts:

- [Name](#naming-fields)
- [Type](#types)
- [Type modifier](#type-modifiers) (optional)
- [Attributes](#attributes) (optional)

You can see examples of fields on the sample models [above](#examples).

### Naming fields

Field names are typically spelled in [camelCase](http://wiki.c2.com/?CamelCase) starting with a lowercase letter.

Technically, a model can be named anything that adheres to this regular expression: 

```
[A-Za-z_][A-Za-z0-9_]*
```

### Types

The type of a field determines its _structure_. A type falls in either of three categories:

- [Scalar type](#scalar-types) (includes [enums](#enums))
- [Model](#models)
- [Embed](#embeds)

### Type modifiers

The type of a field can be modified by appending either of two modifiers:

- `[]`: Make a field a **list** 
- `?`: Make a field **optional**

In the main example above, the field `name` on the `User` model is _optional_ and the `posts` field is a _list_.

Lists can also be optional and will give the list a third state (which is `null`):

- `Blog[]`: Empty list or non-empty list (default: `[]`)
- `Blog[]?`: `null`, empty list or non-empty list (default: `null`)

The default value for a required list is an empty list. The default value for an optional list is `null`.

### Field attributes

Learn more about attributes [below](#attributes).

## Embeds

Embeds are defined via the `embed` blocks in the datamodel and define structures that are _embedded_ in a [model](#models). For a relational database this is often called an _embedded type_, for document databases, an _embedded document_.

Embeds are always included in the [default selection set](./photon/api.md#the-default-selection-set) of the [generated API Photon](./photon/api.md).

### Named embeds

The example [above](#example) defines only one `embed` (called `Address`) which is used exactly once on the `User` model:

```groovy
model User {
  id        Int      @id
  address   Address?
}

embed Address {
  street: String
  zipCode: String
}
```

Named embeds can be reused across multiple models.

### Inline embeds

In the above example, the named embed `Address` is only used once. In this case, it is possible to omit the name and define the `embed` block directly _inline_:

```groovy
model User {
  id        Int      @id
  address   embed {
    street: String
    zipCode: String
  }?
}
```

Inline embeds can also be _nested_.

## Enums

An enum describes a _type_ that has a predefined set of values and is defined via an `enum` block:

```groovy
enum Color {
  Red
  Teal
}
```

You can map the values of an enum to the respective values in the data source:

```
enum Color {
  Red  = "RED"
  Teal = "TEAL"
}
```

Prisma currently only supports string enum value types.

## Type definitions

## Attributes

Attributes modify the behavior of a [field]() or block ([model](), [embed](), ...). There are two ways to add attributes to your data model:

- [Field-level attributes]() are prefixed with `@`.
- [Block-level attributes]() are prefixed with `@@`.

Depending on their signature, attributes may be called in the following cases:

**Case 1. No arguments**

- _Signature_: `@attribute`
- _Description_: Parenthesis **must** be omitted.
- _Examples_:
  - `@id`
  - `@unique`
  - `@updatedAt`

**Case 2. One positional argument**

- _Signature_: `@attribute(_ p0: T0, p1: T1, ...)`
- _Description_: There may be up to one positional argument that doesn't need to be named.
- _Examples_:
  - `@field("my_column")`
  - `@default(10)`
  - `@createdAt(now())`

For arrays with a single parameter, you **may** omit the surrounding brackets:

```groovy
@attribute([email]) // is the same as
@attribute(email)
```

**Case 3. Many named arguments**

- _Signature_: `@attribute(_ p0: T0, p1: T1, ...)`
- _Description_: There may be any number of named arguments. If there is a positional argument, then it **may** appear anywhere in the function signature, but if it's present and required, the caller **must** place it before any named arguments. Named arguments may appear in any order.
- _Examples_:
  - `@@pg.index([ email, first_name ], name: "my_index", partial: true)`
  - `@@pg.index([ first_name, last_name ], unique: true, name: "my_index")`
  - `@@check(a > b, name: "a_b_constraint")`
  - `@pg.numeric(precision: 5, scale: 2)`

You **must not** have multiple arguments with the same name:

```groovy
// compiler error
@attribute(key: "a", key: "b")
```

For arrays with a single parameter, you **may** omit the surrounding brackets:

```groovy
@attribute([item], key: [item]) // is the same as
@attribute(item, key: item)
```

### Field-level attributes

Field attributes are marked by an `@` prefix placed at the _end_ of the field definition. A field can have any number of field arguments, potentially spanning multuple lines.

```
// A field with one attribute
model _ {
  myField String @attribute
}

// A field with two attributes
embed _ {
  myField String @attribute @attribute2
}

// A type definition with three attributes
type MyType String @attribute("input")
         @attribute2("input", key: "value", key2: "value2")
         @attribute3
```

#### Core

_Core_ field attributes must be implemented by every [data source]() connector (with a _best-effort implementation_), this means they will be available in _any_ Prisma project.

They may be used in `model` and `embed` blocks as well as on `type` definitions

Here is a list of all available core field attributes:

- `@id`: Defines the primary key.
- `@unique`: Defines a unique constraint.
- `@map(\_ name: String)`: Defines the raw column name the field is mapped to.
- `@default(\_ expr: Expr)`: Specifies a default value.
- `@relation(\_ fields?: Identifier[], name?: String, onDelete?: CascadeEnum)`: Disambiguates relationships when needed. More details [here](#the-relation-attribute).
- `@updatedAt`: Updates the time to `now()` whenever a record is updated.

#### Connector

_Connector_ field attributes let you use the native features of your data source. With a PostgreSQL database, you can use it for example to X.

### Block-level attributes

Block-level attributes are marked by an `@@` prefix placed anywhere inside a block. You can have as many block attributes as you want and they may also span multiple lines:

```
model \_ { @@attribute0

---

@@attribute1("input") @attribute2("input", key: "value", key2: "value2")

---

@@attribute3 }

embed \_ { @@attribute0

---

@@attribute1 @@attribute2("input") }
```


### Block-level attributes

## Functions

## Scalar types

## Relations