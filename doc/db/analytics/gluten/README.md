---
title: Gluten
keywords: [gluten, spark, velox, clickhouse, native-execution, oap, intel]
status: reviewed
---

# Gluten

[Apache Gluten](https://gluten.apache.org/) (originally an Intel OAP project,
now incubating at Apache) is a middle-layer that offloads Spark SQL operators
to native execution engines — primarily [Velox](https://velox-lib.io/) and
[ClickHouse](https://clickhouse.com/). It plugs in below Catalyst as a Spark
plugin and rewrites supported physical operators into native calls,
dramatically improving CPU efficiency for SQL-heavy workloads while keeping
the Spark API surface intact.

This folder holds two early Intel OAP-project decks that frame the
architecture and motivation.

## What's in this folder

- `gluten_01a.pptx` — early architecture / "what is Gluten" deck.
- `OAP_project_gluten.pptx` — Intel OAP-project framing: how Gluten fits with
  the rest of the OAP optimization suite.

## External entry points

- [gluten.apache.org](https://gluten.apache.org/)
- [github.com/apache/incubator-gluten](https://github.com/apache/incubator-gluten)
- [Velox](https://velox-lib.io/) — the C++ execution engine Gluten most
  commonly targets.
- [Intel OAP](https://github.com/oap-project) — the GitHub umbrella for the
  origin project.

## Cross-section see-also

- [[db/analytics/datafusion/README|DataFusion]] — the Rust/Arrow alternative
  to native Spark offload.
- [[programming/rust/data/datafusion/gluten|gluten (code notes)]] — code-side
  notes on the same project.
- [[db/analytics/README|analytics]] — sibling engine notes.

## Keywords

`#gluten` `#spark` `#velox` `#clickhouse` `#native-execution` `#oap`
