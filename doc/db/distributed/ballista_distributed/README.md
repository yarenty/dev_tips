---
title: Ballista (distributed DataFusion)
keywords: [ballista, datafusion, distributed-query, arrow, rust]
status: reviewed
review_date: 2026/05/03
---

# Ballista (distributed DataFusion)

[Apache Ballista](https://github.com/apache/datafusion-ballista) is the
distributed query execution engine on top of [[db/analytics/datafusion/README|DataFusion]].
It takes a DataFusion physical plan and runs it across a cluster of executor
nodes coordinated by a scheduler — broadly the same architectural slot Spark
occupies, but Rust/Arrow native and dramatically lighter weight.

## External entry points

- [github.com/apache/datafusion-ballista](https://github.com/apache/datafusion-ballista)
- [Ballista user guide](https://datafusion.apache.org/ballista/) (lives
  under the DataFusion site)
- [Apache Arrow Flight](https://arrow.apache.org/docs/format/Flight.html) —
  the RPC transport Ballista uses for shuffle.

## Cross-section see-also

- [[db/analytics/datafusion/README|DataFusion]] — the single-node engine
  Ballista distributes.
- [[db/distributed/scheduling/README|scheduling]] — the broader
  scheduler-research context.
- [[db/distributed/README|distributed]] — sibling distributed-DB topics.

## Keywords

`#ballista` `#datafusion` `#distributed-query` `#arrow` `#rust`
