# Bam Refactor API

This repository is intended to house my API plans for the bam refactor. None of
these endpoints are set in stone nor are they actually implemented as of the time
of writing.

Counterparts for some of these endpoints already exist in the v1 RAPTR API, but
since I am hoping to flesh out a v2 API, those endpoints are being redesigned
so that they fit a consistent format.

## The Rules

- If an endpoint is expected to modify and/or return a single record, then the
    name of that record's type should be plural. For example, an endoint to
    create a Sample would be `/sample` but an endpoint to return all Samples
    would be `/samples`.
- An endpoint that will be used to create or update a record should only include
    that record's type. Thus, an endpoint to create or modify a Project should
    be `/project` or `/project/{project_id}.
- The names of parameters should be snake case. For example, `sample_id` as
    opposed to `sampleId`.
- Keys in returned objects should be snake case **UNLESS** the key is for a
    relationship. For example, and endpoint for Suites may include a key for the
    AnlsTypes included in that Suite. That key should be named `anlsTypes`
