# Guidelines/Rules

The following is the current working design rules for this API design. As even
these rules are currently in development, nothing on this page should be
considered set in stone, though the rules herein should align to what is in
the [API documentation](/bam_refactor-api/documentation).

## Small, Atomic Actions

This API is designed to offer individual, atomic actions over more complex,
purpose-built endpoints. Thus, for most actual uses, multiple endpoints will
need to be used. For example, in order to create an Order and assign Bams to it,
the Order should be created along with the Subjects, Samples, and Bams only after
which could the resulting Bams be assigned to the Order.

## Path Naming

- Path names must include the name of the model being created, deleted, updated,
or requested.
- In the case of model names made up of more than one word (e.g., disease code),
  the names of these models must be included in snake case (e.g., disease_code)
  and not in camel case (e.g., diseaseCode) or kebab case (e.g., disease-code).
- Path names must be singular if they are expected to create, delete, update, or
  return a single record. For example, an endpoint to create a Subject would be
  `/subject`
- Path names must be plural if they are expected to create, delete, update, or
  return multiple records. For example, an endpoint to list all Subject records
  would be `/subjects`
- In the case of compound path names (i.e., paths involving the names of more than
  one model), the final model name in the path's plurality must indicate whether
  it interacts with one or more records.
- References to existing records must be made via the primary key for that type
  of record whenever possible. For example, a specific Bam would be referenced
  by its `bam_id`.
- Paths must begin with the name of the model being interacted with. For example,
  if a user is creating a project, the path should begin with `/project`.
  - In the case of smaller models that are additional information to another
    model but not useful in isolation should be accessed via a route beginning
    with the name of the primary model. For example, an endpoint for adding a
    synonym to a Sample would look like `/sample/{sample_id}/synonym`

## HTTP Verbs

- `GET` must only be used when requesting information. This verb should not
  be used to create, update, or destroy a record
- `POST` must be used when creating a record, even records in join tables.
- `PATCH` should be used when modifying a record.
