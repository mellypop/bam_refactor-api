# Schema Changes

## Removing bam_tpl
The biggest change being made to the database schema is the removal of the
`bam_tpl` table. All of the useful columns from that table are being moved to
the `bam` table. These fields are: `sample_target_project_id`, `qualifier`,
`primary_bam_id`, and `legacy`.

Since `primary_bam_id` is a foreign key pointing to the `bam` table, it cannot
be brought over directly and is instead replaced with a new boolean field named
`is_primary`.

## Adding bam_order

Instead of connecting `bam` to `orders` indirectly through
`sample_target_project_order`, a new table is being created: `bam_order`. As
part of adding this table, `sample_target_project_order` will be dropped, and
all of its data will be migrated to `bam_order`

### Potential Concerns

- If either a `bam` or an order (`orders`) does not exist, then it may be difficult
to preserve the connection represented by `sample_target_project_order`.

## ER Diagram
```mermaid
erDiagram
anls_type {
    int anls_type_id PK
    text name UK
    text description
    int ordinal UK
    text blt_item_type
    text bam_pair_role
    bool is_combable
    bool is_mapper
}
anls_type |o--o{ bam : ""

bam {
    int bam_id PK
    int anls_type_id FK
    text bam_status FK
    int gedi_source_id
    int genome_id FK
    int sample_target_project_id FK
    text notes
    text qualifier
    bool is_primary
    bool legacy
}
bam |o--o{ bam_order : ""
bam |o--o{ bam_pair : "case"
bam |o--o{ bam_pair : "control"

bam_order {
    int bam_order_id PK
    int bam_id FK
    int order_id FK
    bool do_analysis
    bool do_nma
}

bam_pair {
    int bam_pair_id PK
    int case_bam_id FK
    int control_bam_id FK
    text qualifier
    text status
    text notes
}

bam_status {
    int bam_status_id PK
    text status UK
    text description
}
bam_status |o--o{ bam : ""

genome {
    int genome_id PK
    int organism_id FK
    text name
    bool is_usable
}
genome |o--o{ bam : ""

orders {
    int id PK
}
orders |o--o{ bam_order : ""

organism {
    int organism_id PK
    text name
    text genus
    text species
}
organism |o--o{ genome : ""

project {
    int project_id PK
    text name
    text subproject
    text username
}
project |o--o{ orders : ""

sample {
    int sample_id PK
    text formal_name
    text pi_name
    text lab_name
}
sample |o--o{ sample_target : ""

sample_target {
    int sample_target_id PK
    int sample_id FK
    int target_id FK
}
sample_target |o--o{ sample_target_project : ""

sample_target_project {
    int sample_target_project_id PK
    int project_id FK
}
sample_target_project |o--o{ bam : ""
```
