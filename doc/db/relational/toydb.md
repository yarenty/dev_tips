---
title: "toyDB — distributed SQL database in Rust, built to be read"
main_link: https://github.com/erikgrinaker/toydb
keywords: [toydb, rust, raft, mvcc, distributed-sql, educational, erik-grinaker]
status: reviewed
review_date: 2026/05/03
---

# toyDB — distributed SQL database in Rust, built to be read

**Main link:** <https://github.com/erikgrinaker/toydb>

Author's blog (CockroachDB / Neon): <https://erikgrinaker.com/>

## Summary

toyDB is an educational distributed SQL database written from scratch in Rust by Erik Grinaker (former CockroachDB and Neon engineer). It's designed to be **simple, complete, and correct** rather than fast — a working example of the architecture and concepts behind a real distributed SQL database. It implements Raft consensus, MVCC snapshot-isolation transactions, a pluggable storage engine (BitCask + in-memory), an iterator-based query engine with heuristic optimisation, and a SQL interface with joins, aggregates, and transactions.

## Insight

This is the best free way to learn how a database actually works. Read the source.

The 2024 rewrite is the version to study — Erik specifically rewrote it after years of building production distributed SQL databases (CockroachDB, Neon) so the structure now reflects how those systems are actually organised, with shortcuts taken intentionally where production complexity would obscure the concept.

What makes it valuable as a learning resource:

- Each layer (storage, MVCC, raft, SQL parser, planner, executor, network) is a small focused module, well under 1000 lines each.
- Real Raft (not a toy variant), real MVCC (not a stub), real query execution.
- Tests are extensive and serve as additional documentation.
- It compiles and runs; you can `cargo run` and play with a real distributed cluster on your laptop.

What it explicitly is not:

- **Production-ready.** Performance, scalability, availability, and operational tooling are non-goals.
- **Feature-complete vs Postgres.** No window functions, limited type system, no extensions.

If you want a more production-shaped reference, look at the [CockroachDB](https://www.cockroachlabs.com/) source, or [Neon](https://neon.tech/) for a Postgres-compatible alternative — both share architectural DNA with toyDB but with all the production complexity put back in.

Other educational DB sources worth pairing with toyDB:

- *Database Internals* by Alex Petrov (book).
- *Designing Data-Intensive Applications* by Martin Kleppmann (book).
- The CMU Database Group lectures by Andy Pavlo (free on YouTube).

## References / raw notes

Headline features (from the README):

- Raft distributed consensus for linearizable state-machine replication.
- ACID transactions with MVCC-based snapshot isolation.
- Pluggable storage engine with BitCask and in-memory backends.
- Iterator-based query engine with heuristic optimisation and time-travel support.
- SQL interface with joins, aggregates, transactions.

> I originally wrote toyDB in 2020 to learn more about database internals. Since then, I've spent several years building real distributed SQL databases at CockroachDB and Neon. Based on this experience, I've rewritten toyDB as a simple illustration of the architecture and concepts behind distributed SQL databases.
>
> toyDB is intended to be simple and understandable, and also functional and correct. Other aspects like performance, scalability, and availability are non-goals — these are major sources of complexity in production-grade databases, and obscure the basic underlying concepts. Shortcuts have been taken where possible.

## Similar / related topics

- [[postgresql]] — the production-grade endpoint of "actually using a SQL DB".
- [[limbo]] — clean-room SQLite rewrite in Rust; smaller scope, also instructive reading.
- [CockroachDB source](https://github.com/cockroachdb/cockroach) — the production-shaped equivalent.
- [TiKV](https://tikv.org/) — distributed KV store also written in Rust, used by [[surrealdb]].
- *Database Internals* (book) by Alex Petrov.

## Internal links

- [[postgresql]]
- [[limbo]]
- [[surrealdb]]

## Keywords

`#toydb` `#rust` `#raft` `#mvcc` `#distributed-sql` `#educational` `#database-internals` `#db`
