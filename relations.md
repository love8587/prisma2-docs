# Relations

This is an extension of the [data modeling](./data-modeling.md) chapter that discusses _relations_ in the data model definition in detail.

The examples on this page are based on this [project file]():


```groovy
// project.prisma

datasource mysql {
  url      = "file:data.db"
  provider = "sqlite"
}

model User {
  id        Int      @id
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
  author     User
  categories Category[]
}

model Category {
  id    Int    @id
  posts Post[]
}

enum Role {
  USER
  ADMIN
}
```

> Note that here all scalars have been removed from the [example data model](./data-modeling.md/#example) so you can focus on the relations.

It contains the following relations:

- 1:1: `User` <-> `Profile`
- 1:n: `User` <-> `Post`
- m:n: `Post` <-> `Category`

## The `@relation` attribute

- `@relation(\_ fields?: Identifier[], name?: String, onDelete?: CascadeEnum)`: Disambiguates relationships when needed.

More info:

- fields: _(optional)_ list of field names to reference
- name: _(optional)_ defines the name of the relationship
- onDelete: _(optional)_ defines what we do when the referenced relation is
  deleted
  - **CASCADE**: also delete this entry
  - **SET_NULL**: set the field to null. This is the default

## 1:1

The return value on both sides is a nullable single value. Prisma prevents accidentally storing multiple records in the relation.

```

```


## 1:n

The return value on one side is a optional single value, on the other side a list that might be empty.

## m:n

The return value on both sides is a list that might be empty. This is an improvement over the standard implementation in relational databases that require the application developer to deal with implementation details such as an intermediate table / join table. In Prisma, each connector will implement this concept in the way that is most efficient on the given storage engine and expose an API that hides the implementation details.

## Self-relations

## Relations in the generated Photon API

