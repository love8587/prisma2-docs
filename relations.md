# Relations

This is an extension of the [data modeling](./data-modeling.md) chapter that discusses _relations_ in the data model definition in detail.

## The `@relation` attribute

- fields: _(optional)_ list of field names to reference
- name: _(optional)_ defines the name of the relationship
- onDelete: _(optional)_ defines what we do when the referenced relation is
  deleted
  - **CASCADE**: also delete this entry
  - **SET_NULL**: set the field to null. This is the default

## 1:1

## 1:n

## m:n

## Self-relations

## Relations in the generated Photon API

