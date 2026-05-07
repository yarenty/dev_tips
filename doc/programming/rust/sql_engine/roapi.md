---
title: ROAPI — read-only API server over any data source
main_link: https://github.com/roapi/roapi
keywords: [roapi, datafusion, arrow, sql-api, graphql-api, rest-api, columnq]
status: reviewed
---

# ROAPI — read-only API server over any data source

**Main link:** <https://github.com/roapi/roapi>

## Summary

ROAPI ("Read-Only API") spins up a single-binary server that exposes any static dataset (CSV / JSON / Parquet / Delta / files on local disk / S3 / GCS / Azure / a SQL DB) as a queryable REST + GraphQL + SQL endpoint, with **zero application code**. Architecturally it is a thin wrapper over [Apache DataFusion](../data/datafusion/README.md) plus Apache Arrow: query frontends translate SQL / GraphQL / REST into DataFusion logical plans, DataFusion executes, and a response-encoding layer serialises the resulting Arrow `RecordBatch` into JSON / Arrow IPC / Parquet for the client.

## Insight

The killer use case is "I have a Parquet file (or a folder of CSVs, or a Delta table on S3) and I want a real SQL+REST+GraphQL API in front of it for an internal tool, dashboard, or LLM agent" — ROAPI gets you from zero to served in one config file (TOML or YAML) declaring `tables: [...]`. Schema is auto-inferred (with optional override). Don't reach for it when: (a) you need writes (it really is read-only — there is sister project `columnq-cli` for one-shot queries, but writes are out of scope by design); (b) you need fine-grained authn/authz (it has basic-auth and JWT but no row-level policy); (c) you need to serve > tens-of-millions of rows interactively (it is single-process; for cluster scale you'd reach for Trino/Presto, Spark Thrift, or Ballista). The mental model: ROAPI is to DataFusion roughly what PostgREST is to Postgres — instant API on top of an existing data substrate. Pairs nicely with **Arrow Flight SQL** and modern Polars / Pandas Arrow-native consumers, since Arrow IPC is one of the supported response formats.

## Similar / related topics

- **PostgREST** — same idea but for Postgres specifically; the design ROAPI clearly echoes.
- **Hasura / PostGraphile** — GraphQL-on-Postgres; richer but not file-source-aware.
- **Trino / Presto** — the JVM big-cousin; cluster-scale federation.
- **Apache Drill** — older "SQL on files" engine; same niche, less alive.
- [[datafusion]] — the engine ROAPI sits on top of.

## Internal links

<!-- reviewed -->
- [[README]]
- [[datafusion]] — the planner.
- [[../data/datafusion/README|DataFusion landing]]
- [[../data/datafusion/delta|delta-rs]] / `iceberg-rust` — table-format sources ROAPI can read.
- [[../net/grpc|tonic]] — Arrow Flight SQL is a tonic-shaped story if you want lower-latency than HTTP+JSON.

## Keywords

`#roapi` `#datafusion` `#arrow` `#sql-api` `#graphql-api` `#rest-api`

## References / raw notes

- Source: <https://github.com/roapi/roapi>
- Docs: <https://roapi.github.io/docs/>
- GraphQL implementation (the file originally linked here): <https://github.com/roapi/roapi/blob/main/columnq/src/query/graphql.rs>

> ROAPI automatically spins up read-only APIs for static datasets without requiring you to write a single line of code. It builds on top of Apache Arrow and DataFusion. Core design:
>
> - Query frontends translate SQL, GraphQL and REST API queries into DataFusion plans.
> - DataFusion executes the plan.
> - A data layer loads datasets from a variety of sources and formats with automatic schema inference.
> - A response-encoding layer serialises the intermediate Arrow record batch into the format requested by the client.

![ROAPI architecture](https://camo.githubusercontent.com/6846b7b40ddfc780fc97c2238fd7a4ea2594bde8883adf30fb0dc8086185c80d/68747470733a2f2f726f6170692e6769746875622e696f2f646f63732f696d616765732f726f6170692e706e67)
