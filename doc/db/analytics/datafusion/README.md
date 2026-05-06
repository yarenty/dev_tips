---
title: DataFusion (analytics notes)
keywords: [datafusion, arrow, rust, query-engine, ballista, migration]
status: reviewed
---

# DataFusion (analytics notes)

[Apache DataFusion](https://datafusion.apache.org/) is the Rust query engine
that lives under the Apache Arrow umbrella. It compiles SQL and DataFrame
expressions into a physical plan over columnar Arrow batches and is used as
the embedded engine for projects like Databend, InfluxDB 3.0, ROAPI, Comet,
and many others.

This folder holds working decks from earlier internal evaluations — gap
analysis vs Spark, concurrent-write design, migration planning, and an
RDD-to-SQL paper. For a code-side view of DataFusion (extension hooks, blaze,
delta, gluten, iceberg, vortex, etc.), see
[[programming/rust/data/datafusion/README|the DataFusion code notes folder]].

## What's in this folder

- `Concurrent Writes.pptx` — design notes for handling concurrent writes
  through DataFusion / object-store backends.
- `Datafusion and Ballista Gaps & Estimations.pptx` — feature-gap analysis
  vs Spark, with estimation effort for closing the gaps.
- `Migration.pptx` — migration planning deck (Spark → DataFusion / Ballista).
- `RDD2SQL.pdf` — research/whitepaper on rewriting Spark RDD pipelines as SQL
  to push more work into the optimizer.

## External entry points

- [datafusion.apache.org](https://datafusion.apache.org/)
- [github.com/apache/datafusion](https://github.com/apache/datafusion)
- [DataFusion blog](https://datafusion.apache.org/blog/)
- [Apache Arrow](https://arrow.apache.org/) — the columnar memory format
  underneath.

## Cross-section see-also

- [[db/distributed/ballista_distributed/README|Ballista]] — distributed
  execution engine on top of DataFusion (the "gaps" deck above is largely
  about Ballista vs Spark).
- [[db/analytics/databend/README|Databend]] — Rust DW that reuses parts of
  DataFusion.
- [[db/analytics/gluten/README|Gluten]] — Spark operator offload that
  partly overlaps with the migration story.
- [[programming/rust/data/datafusion/README|DataFusion code notes]] — the
  matching code-side folder.

## Keywords

`#datafusion` `#arrow` `#rust` `#query-engine` `#ballista` `#migration`
