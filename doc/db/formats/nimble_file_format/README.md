---
title: Nimble file format
main_link: https://github.com/facebookincubator/nimble
keywords: [nimble, columnar, file-format, meta, facebook, velox, orc, parquet]
status: reviewed
review_date: 2026/05/03
---

# Nimble file format

**Main link:** <https://github.com/facebookincubator/nimble>

## Summary

Nimble is a new columnar file format developed by Meta as a unified successor
to ORC, DWRF (Meta's internal ORC fork), and to some extent Parquet for
internal use. It's designed for very wide schemas, deeply nested types, and
modern columnar engines — particularly **Velox** (Meta's C++ vectorised
execution library that backs Presto, Spark via Gluten, and others). The
project is maintained under `facebookincubator` on GitHub.

## Insight

Reach for Nimble awareness, not adoption (yet):

- **Why it exists**: Parquet's encoding choices are getting old, ORC has
  Hadoop-era baggage, and Meta needed something tuned for its own scale of
  wide / nested feature data and ML training. Nimble is that something.
- **Where it fits**: tightly coupled to **Velox**. Outside the Velox
  ecosystem (Presto/PrestoDB, Spark+Gluten, DuckDB experiments, some
  internal Meta tools), there is little reason to write Nimble files today.
- **Why care anyway**: it's a useful pointer to where columnar formats are
  heading — better encoding flexibility, schema evolution that doesn't fight
  you, less Hive baggage. Watch it the way you'd watch any
  Meta-incubator-stage spec.
- **Against**: the broader OSS ecosystem is consolidating on Parquet +
  Iceberg/Delta. Nimble has to clear a high bar of momentum before being a
  reasonable default.

## Similar / related topics

- **Apache Parquet** — the reigning default columnar format.
- **Apache ORC** — the other classical columnar format; what Nimble most
  directly replaces internally at Meta.
- **Apache Arrow** — in-memory columnar standard; complementary, not a
  competitor.
- **Velox** — <https://velox-lib.io/> — Meta's vectorised execution library;
  Nimble's primary consumer.
- **Lance** — <https://lancedb.github.io/lance/> — another modern columnar
  format aimed at ML / vector workloads.

## Internal links

- [[db/formats/README|formats]] — section landing.
- [[db/formats/table_transfer_protocols/README|Table transfer protocols]] —
  the wire-format counterpart.
- [[db/analytics/README|analytics]] — Velox underpins analytics engines.

## Keywords

`#nimble` `#columnar` `#file-format` `#meta` `#velox` `#orc` `#parquet`

## References / raw notes

- Repo: <https://github.com/facebookincubator/nimble>
- Velox project: <https://velox-lib.io/>
