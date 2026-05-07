---
title: How to build a SQL engine — reading list
main_link: https://howqueryengineswork.com/
keywords: [sql-engine, books, reading-list, datafusion, query-engine, andy-grove]
status: reviewed
---

# How to build a SQL engine — reading list

**Main link:** <https://howqueryengineswork.com/>

## Summary

A small reading list for "how to build a SQL / query engine yourself" — the closest thing to a syllabus pointer for the rest of this section. The headline is Andy Grove's *How Query Engines Work* book ([HTML](https://howqueryengineswork.com/), [GitHub source](https://github.com/andygrove/how-query-engines-work)), which walks through building a Kotlin/JVM query engine end-to-end (parser → logical plan → optimizer → physical plan → execution); the design is the direct ancestor of [Apache DataFusion](../data/datafusion/README.md), which Andy also created.

## Insight

Read this first if you want to understand DataFusion's internals, or if you want to start building your own engine and need a working mental model. The Kotlin choice in the book is a feature, not a bug — it removes the borrow checker and Arrow noise so you can focus on plan-tree shapes; once the concepts click, the Rust translation in DataFusion source is straightforward. Pair it with the canonical academic / industry references below — *Database Internals* (Petrov) for storage-layer design, the CMU 15-445 / 15-721 courses (Andy Pavlo, free on YouTube) for the systems view, and *Designing Data-Intensive Applications* (Kleppmann) for the distributed-systems framing that the engine eventually has to live inside.

## Similar / related topics

- *Database Internals* by Alex Petrov — storage engines (B-tree / LSM), replication, distributed transactions; the book the SQL-engine book doesn't try to be.
- CMU 15-445 (Intro to DBMS) and 15-721 (Advanced DBMS) — Andy Pavlo's free courses; the canonical academic syllabus, with project assignments that build a real OLTP/OLAP engine.
- *Designing Data-Intensive Applications* by Martin Kleppmann — the systems-context book; assumes the engine and asks where it sits.
- [The Morsel-Driven Parallelism paper](https://db.in.tum.de/~leis/papers/morsels.pdf) (HyPer / Leis et al.) — the modern execution-model reference; cited by DataFusion / DuckDB / Velox.
- *Readings in Database Systems* ("the Red Book", Stonebraker / Hellerstein) — annotated curated paper list; out of copyright at <http://www.redbook.io/>.

## Internal links

- [[README]] — the rest of this section: real engines, parsers, UDFs.
- [[../data/datafusion/README]] — the engine the headline book teaches you the bones of.
- [[datafusion]] — DataFusion's SQL planner specifically (the chapter the book ends at).
- [[../../../db/udf/README|db/udf section]] — UDF semantics across engines.
- [[../learning/_to_learn|Rust learning reading list]] — companion reading list for the Rust-specific angle.

## Keywords

`#sql-engine` `#books` `#reading-list` `#datafusion` `#query-engine` `#andy-grove`

## References / raw notes

- *How Query Engines Work* (Andy Grove): <https://howqueryengineswork.com/>
- Source code (Kotlin): <https://github.com/andygrove/how-query-engines-work>
- Andy Grove also created [Apache DataFusion](https://github.com/apache/datafusion) and [Apache Ballista](https://github.com/apache/datafusion-ballista) — this book is the conceptual prequel to both.
- CMU DB courses (free, with assignments): <https://db.cs.cmu.edu/courses/>
- Red Book (Stonebraker / Hellerstein): <http://www.redbook.io/>
