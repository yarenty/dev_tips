---
title: Lance — modern columnar format for ML
main_link: https://github.com/lancedb/lance
keywords: [lance, columnar, parquet, vector, arrow, ml, lancedb]
status: reviewed
---

# Lance — modern columnar format for ML

**Main link:** <https://github.com/lancedb/lance>

## Summary

Lance is a modern columnar data format developed by **LanceDB** as an open-source alternative to Apache Parquet, specifically optimised for ML workflows: 100× faster random access than Parquet, native vector index for ANN search, automatic data versioning (zero-copy "git for data"), and ecosystem integration with Apache Arrow, pandas, Polars, DuckDB, and PyArrow. Conversion from Parquet is a two-line operation. Lance is also the on-disk format for the LanceDB vector database.

## Insight

Reach for Lance when (a) you need **random access into ML datasets** (training loops, feature stores, image/video lookup) where Parquet's row-group seek model is too coarse, (b) you want vector search collocated with the columnar data instead of in a separate vector DB, or (c) you want zero-copy versioning of huge datasets without a `dvc`/git-LFS overlay. Position vs Parquet: Parquet is still the right answer for batch analytics on cold S3 data; Lance wins when access pattern is "single-row-or-small-batch random reads at training time". Position vs LanceDB: LanceDB *uses* Lance as its file format and adds a database layer (table management, transactions, the LanceDB SaaS) on top — you can use Lance the format without LanceDB the database. Gotchas: file format is still pre-1.0 (forwards-compat is a moving target); the "100× faster than Parquet" claim is for random access only — sequential scans are competitive but not a step-change; ecosystem support outside Python/Rust is thin compared to Parquet.

## Similar / related topics

- Apache Parquet — the incumbent columnar format Lance directly challenges.
- Apache Arrow — in-memory format Lance interoperates with.
- LanceDB — vector DB built on Lance the format.
- Nimble — Meta's experimental columnar format (Velox-side).
- DuckDB — popular consumer of Lance via its Arrow integration.
- [[adbc]] — generic Arrow-native consumption story for Lance datasets.

## Internal links
- [[../../../db/formats/README|DB formats landing]]
- [[adbc]]
- [[datafusion/README|DataFusion]]
- [[../../../db/vector/README|Vector databases]]

## Keywords

`#lance` `#columnar` `#parquet` `#vector` `#arrow` `#ml`

## References / raw notes

- Repo: <https://github.com/lancedb/lance>
- Format spec: <https://lancedb.github.io/lance/format.html>
- LanceDB (database built on Lance): <https://lancedb.com/>

> Modern columnar data format for ML. Convert from Parquet in 2 lines of code for 100× faster random access, a vector index, data versioning, and more. Compatible with pandas, DuckDB, Polars, and pyarrow.

Lance is perfect for:

- Building search engines and feature stores.
- Large-scale ML training requiring high-performance IO and shuffles.
- Storing, querying, and inspecting deeply nested data for robotics or large blobs (images, point clouds, video frames).

Key features: high-performance random access, vector search (combine OLAP queries with kNN), zero-copy automatic versioning, integrations with the Arrow ecosystem.
