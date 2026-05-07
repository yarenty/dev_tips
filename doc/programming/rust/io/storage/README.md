---
title: "Rust storage — object stores, lakehouse formats (stub)"
keywords: [rust, storage, object-store, opendal, parquet, deltalake, iceberg, s3, hdfs]
status: reviewed
review_date: 2026/05/03
---

# Rust storage — object stores, lakehouse formats (stub)

This subsection is currently **empty** of dedicated articles — it exists as a parking spot for future write-ups on the Rust storage / object-store layer. The canonical material today lives upstream and in adjacent sections.

## Canonical external pointers

- [`object_store`](https://crates.io/crates/object_store) — Apache Arrow's unified object-store crate. Single async API over AWS S3, Azure Blob, GCS, local filesystem, in-memory, HTTP/WebDAV, plus extensions for HDFS. The de-facto choice in Rust analytics engines (DataFusion, delta-rs, Lance, Arroyo, Ballista).
- [Apache OpenDAL](https://opendal.apache.org/) — broader cross-language data-access layer. Rust core (`opendal` crate) with bindings for Python / Java / Node / Go. ~50 storage backends including HDFS, IPFS, FTP, GitHub, Hugging Face, Supabase. Originated at DatabendLabs, ASF top-level project as of 2024.
- [`hdfs-object-store`](https://crates.io/crates/hdfs-object-store) — `object_store` backend built on [`hdfs-native`](https://crates.io/crates/hdfs-native) (pure-Rust HDFS client).
- [`delta-rs`](https://github.com/delta-io/delta-rs) — pure-Rust Delta Lake; ACID + time-travel on top of Parquet via `object_store`.
- [`iceberg-rust`](https://github.com/apache/iceberg-rust) — Apache Iceberg native Rust impl (catalog, table format, manifest readers); much newer than the Java/Python references but moving fast.
- [`lance`](https://github.com/lancedb/lance) — modern columnar format optimised for ML / vector workloads; ships its own object-store-shaped IO.
- [`parquet`](https://crates.io/crates/parquet) — Apache Arrow's pure-Rust Parquet reader/writer; usually consumed via DataFusion or Polars rather than directly.

## When this gets filled

Once any of the above grows substantive vault content (e.g. a write-up on `object_store` patterns, an OpenDAL recipe, or a Delta Lake gotchas page), drop a stem under `programming/rust/io/storage/` and link from this README.

## See also

- [[../README|Rust I/O]] — parent section.
- [[../hdfs|HDFS clients in Rust]] — the legacy-filesystem sibling article.
- [[db/formats/README|db/formats]] — Parquet / ORC / Iceberg / Avro / Nimble.
- [[db/catalogs/README|db/catalogs]] — Unity / Hive / Polaris catalogs that sit above lakehouse formats.
- [[db/distributed/datafusion/README|DataFusion]] — biggest single consumer of `object_store` in the Rust ecosystem.

## Keywords

`#rust` `#storage` `#object-store` `#opendal` `#parquet` `#deltalake` `#iceberg` `#s3` `#hdfs`
