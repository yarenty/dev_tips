---
title: Lance
main_link: https://github.com/lancedb/lance
keywords: [lance-data-format, rust, format, parquet, vector, arrow, search, apache]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Lance

**Main link:** <https://github.com/lancedb/lance>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[adbc]] — ADBC ! _(score 23.4)_
- [[memory_storage]] — Memory storage _(score 17.1)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 17.1)_
- [[barrel]] — barrel _(score 17.1)_
- [[apache]] — apache _(score 17.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#lance-data-format` `#data` `#rust` `#programming` `#format` `#parquet` `#vector` `#duckdb`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Lance

https://github.com/lancedb/lance




Modern columnar data format for ML. Convert from Parquet in 2-lines of code for 100x faster random access, a vector index, data versioning, and more.
Compatible with pandas, DuckDB, Polars, and pyarrow with more integrations on the way.


Lance is a modern columnar data format that is optimized for ML workflows and datasets. Lance is perfect for:

Building search engines and feature stores.
Large-scale ML training requiring high performance IO and shuffles.
Storing, querying, and inspecting deeply nested data for robotics or large blobs like images, point clouds, and more.
The key features of Lance include:

High-performance random access: 100x faster than Parquet without sacrificing scan performance.

Vector search: find nearest neighbors in milliseconds and combine OLAP-queries with vector search.

Zero-copy, automatic versioning: manage versions of your data without needing extra infrastructure.

Ecosystem integrations: Apache Arrow, Pandas, Polars, DuckDB and more on the way.



## format


https://lancedb.github.io/lance/format.html
