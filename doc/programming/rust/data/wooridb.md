---
title: WooriDB — embedded temporal NoSQL DB
main_link: https://github.com/naomijub/wooridb
keywords: [wooridb, temporal, datalog, schemaless, experimental]
status: reviewed
review_date: 2026/05/03
---

# WooriDB — embedded temporal NoSQL DB

**Main link:** <https://github.com/naomijub/wooridb>

## Summary

WooriDB ("우리" = Korean for "our") is an experimental general-purpose **temporal** NoSQL database in Rust by Julia Naomi, inspired by Crux, Datomic, Prometheus, and SparQL. It is schemaless key-value storage where every entity is automatically indexed by `(entity_name, datetime, uuid)`, supports a Datalog-flavoured query syntax, and persists locally. Niche features include hashed-and-checked fields (`ENCRYPT` + `CHECK`), conditional updates, entity history, RON-typed I/O, and arbitrary-precision numbers via a `P` suffix.

## Insight

Honest framing: **WooriDB is a low-activity hobby project**, not production-ready software. It's interesting as a worked example of a Datomic-flavoured store implemented in idiomatic Rust, and as study material for someone exploring temporal-DB design — see the project's reading list (Database Internals, DDIA, Pavlo's lectures). Don't deploy it; do read the code if you want to understand how the bitemporal model from Datomic and the "everything is a fact at a time" idea translate into Rust types. For a real production temporal DB, look at XTDB (Clojure, Datomic's spiritual successor) or Datomic itself; for time-series specifically use [[greptimedb]] / InfluxDB / TimescaleDB.

## Similar / related topics

- Crux / XTDB — the Clojure inspiration; production-grade bitemporal DB.
- Datomic — the Rich Hickey-led commercial original.
- [[greptimedb]] — Rust time-series DB (different niche, real production).
- [[gluesql]] — embedded SQL engine; closer to "actually use this" territory.
- [[surrealdb]] — multi-model embedded DB.

## Internal links
- [[gluesql]]
- [[surrealdb]]
- [[greptimedb]]
- [[../../../db/timeseries/README|Time-series databases]]

## Keywords

`#wooridb` `#temporal` `#datalog` `#schemaless` `#experimental`

## References / raw notes

- Repo: <https://github.com/naomijub/wooridb>

WooriDB is a general-purpose **experimental** time-serial database, meaning it contains all entity registries indexed by DateTime. It is schemaless key-value storage and uses its own query syntax similar to SparQL and Crux's Datalog.

Features:

- Hashing keys with `ENCRYPT`. Hashed values filtered out, checkable only with `CHECK`.
- Ron schemas for input/output; JSON via feature flag.
- Entities indexed by entity_name (Entity Tree), DateTime (Time Serial), and UUID (Entity ID).
- Persistent local storage.
- Arbitrary-precision numbers via the `P` suffix.
- Configuration via environment variables.
- Authentication / authorization via session token.
- Conditional update.
- Some relational algebra.
- Entity history.

Inspired by Crux, Datomic, Prometheus, SparQL, *Database Internals*, *Database System Concepts*, *Designing Data-Intensive Applications*, Andy Pavlo's database lectures, and "Zero Trust in Time Series Data?".
