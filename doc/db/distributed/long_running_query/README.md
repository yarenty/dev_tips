---
title: Long-running query handling (placeholder)
keywords: [long-running-query, checkpointing, fault-tolerance, restart, query-execution]
status: reviewed
---

# Long-running query handling (placeholder)

Notes folder for handling **queries that run for hours** — the failure modes
and design choices that don't matter for sub-second OLTP but completely
dominate batch ETL and analytical workloads. The core problems:

- **Fault tolerance** — a single executor failure 90% of the way through a
  10-hour query shouldn't restart the whole thing; intermediate state needs
  to be checkpointed.
- **Restartability** — given a checkpoint, the query needs to resume from
  the last consistent point without re-reading already-processed input.
- **Resource management** — long queries hold onto memory, scratch disk, and
  shuffle storage; spilling and garbage collection of intermediate state
  matters more than peak performance.
- **Observability** — operators need progress estimates and per-stage
  metrics, not just "the query is still running."

The Spark / MapReduce lineage solves this with stage-level recomputation
from materialized shuffle output; modern engines (Trino with fault-tolerant
execution, Snowflake) have variations on the same theme.

## External entry points

- [Trino fault-tolerant execution](https://trino.io/docs/current/admin/fault-tolerant-execution.html)
- [Apache Spark — RDD lineage / fault recovery](https://spark.apache.org/docs/latest/rdd-programming-guide.html)

## Cross-section see-also

- [[db/distributed/scheduling/README|scheduling]] — checkpointing and
  preemption are scheduler-side concerns.
- [[db/distributed/ballista_distributed/README|ballista_distributed]] —
  the Rust/Arrow distributed engine where this story is still actively
  evolving.
- [[db/distributed/README|distributed]] — parent section.

## Keywords

`#long-running-query` `#checkpointing` `#fault-tolerance` `#restart` `#query-execution`
