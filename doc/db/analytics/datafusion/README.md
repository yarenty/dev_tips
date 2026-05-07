---
title: DataFusion (analytics notes)
keywords: [datafusion, arrow, rust, query-engine, ballista, migration]
status: reviewed
review_date: 2026/05/03
---

# DataFusion (analytics notes)

[Apache DataFusion](https://datafusion.apache.org/) is the Rust query engine
that lives under the Apache Arrow umbrella. It compiles SQL and DataFrame
expressions into a physical plan over columnar Arrow batches and is used as
the embedded engine for projects like Databend, InfluxDB 3.0, ROAPI, Comet,
and many others.

This folder collects working notes from earlier internal evaluations — gap
analysis vs Spark, concurrent-write design, migration planning, and an
RDD-to-SQL exploration. For a code-side view of DataFusion (extension hooks,
blaze, delta, gluten, iceberg, vortex, etc.), see
[[programming/rust/data/datafusion/README|the DataFusion code notes folder]].

## External entry points

- [datafusion.apache.org](https://datafusion.apache.org/)
- [github.com/apache/datafusion](https://github.com/apache/datafusion)
- [DataFusion blog](https://datafusion.apache.org/blog/)
- [Apache Arrow](https://arrow.apache.org/) — the columnar memory format
  underneath.

## Cross-section see-also

- [[db/distributed/ballista_distributed/README|Ballista]] — distributed
  execution engine on top of DataFusion.
- [[db/analytics/databend/README|Databend]] — Rust DW that reuses parts of
  DataFusion.
- [[db/analytics/gluten/README|Gluten]] — Spark operator offload that
  partly overlaps with the migration story.
- [[programming/rust/data/datafusion/README|DataFusion code notes]] — the
  matching code-side folder.

## Keywords

`#datafusion` `#arrow` `#rust` `#query-engine` `#ballista` `#migration`
