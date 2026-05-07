---
title: Vortex — modern compressed columnar file format
main_link: https://github.com/spiraldb/vortex
keywords: [vortex, spiral, columnar, parquet, arrow, compression, file-format, fastlanes, alp]
status: reviewed
---

# Vortex — modern compressed columnar file format

**Main link:** <https://github.com/spiraldb/vortex>

## Summary

**Vortex** is a Rust toolkit and aspiring file format for compressed Apache Arrow arrays — in-memory, on-disk, and over-the-wire — built by [Spiral](https://spiraldb.com/). The pitch is to be **what DataFusion is to query engines, or what LLVM+Clang are to compilers**: a highly extensible, batteries-included framework for building modern columnar file formats. The headline claim is "aspiring successor to Apache Parquet" with **100-200× faster random-access reads** and **2-10× faster scans**, while keeping comparable compression ratios and write throughput. It supports very wide tables (10s of thousands of columns) and aims at on-device GPU decompression.

## Insight

Vortex sits at the **file-format** layer (one below the table-format layer occupied by [[delta|Delta Lake]] / [[iceberg|Iceberg]] / [[lakesoul|LakeSoul]]). The contemporaries to compare against:

| Format | Vendor | Distinguishing trait |
|---|---|---|
| **Apache Parquet** | community / ASF | The incumbent; columnar, ~20-year-old design heritage from Trevni/Dremel |
| **Vortex** | Spiral | Pluggable encodings (FastLanes, ALP, FSST, BtrBlocks); fast random access; GPU-aware design |
| **Lance** | LanceDB | ML-first columnar format; great random access for vector / image columns |
| **Nimble** | Meta | Velox-side replacement for Parquet inside Meta's stack |
| **Apache ORC** | community / ASF | Hive-lineage Parquet sibling, less general adoption |

What makes Vortex architecturally interesting:

- **Logical types separate from physical layout** — schema definition makes no claim about encoding; encoder picks per column.
- **Cascading + pluggable compression** — FastLanes, ALP, FSST, BtrBlocks-style strategies, recursively composable.
- **Zero-copy to / from Arrow** — "canonicalised" Vortex arrays are bit-equivalent to Arrow.
- **Compute on encoded data** — basic kernels (filter pushdown) operate without full decompression.
- **Lazy statistics** — each array carries summary stats, available to compressors and to query planners.
- **GPU decompression on the roadmap** — the small intentional set of data-parallel encodings is chosen with vectorised / SIMD / GPU paths in mind.

When to reach for it (today, late 2024 / 2025): you're **building a new system** and Parquet's random-access cost is your bottleneck (analytics over many small lookups, vector-DB-shaped workloads, ML feature stores). When *not* to: anything that has to interoperate with the existing Parquet ecosystem (Spark, Hive, Trino, Snowflake, BigQuery), where Vortex isn't yet a recognised input. The format is young; expect rough edges and breaking changes.

## Similar / related topics

- **Apache Parquet** — the incumbent file format Vortex aims to succeed.
- [[../lance_data_format|lance]] — LanceDB's ML-first columnar format, similar generation.
- **Nimble (Meta)** — Velox-side Parquet replacement.
- **FastLanes / ALP / FSST / BtrBlocks** — modern compression encodings Vortex bundles.
- **Apache Arrow** — the in-memory format Vortex zero-copies to/from.

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[../lance_data_format|lance]] — sibling modern columnar format (ML-first).
- [[delta]] — table-format layer above (uses Parquet today).
- [[iceberg]] — table-format layer above (uses Parquet/ORC/Avro today).
- [[../../../../db/formats/README|db/formats]] — broader format landing.
- [[../../../../db/formats/nimble_file_format/README|nimble]] — Meta's Velox-side Parquet replacement.

## Keywords

`#vortex` `#spiral` `#columnar` `#parquet` `#arrow` `#compression` `#file-format` `#fastlanes` `#alp`

## References / raw notes

- Repo: <https://github.com/spiraldb/vortex>
- Spiral (the company): <https://spiraldb.com/>
- BtrBlocks (the reference compression strategy): <https://github.com/maxi-k/btrblocks>
- FastLanes (one of the encodings): <https://github.com/cwida/FastLanes>

> Vortex is designed to be to columnar file formats what Apache DataFusion is to query engines (or, analogously, what LLVM + Clang are to compilers): a highly extensible & extremely fast framework for building a modern columnar file format, with a state-of-the-art, "batteries included" reference implementation.
>
> Vortex is an aspiring successor to Apache Parquet, with dramatically faster random access reads (100-200× faster) and scans (2-10× faster), while preserving approximately the same compression ratio and write throughput. It will also support very wide tables (at least 10s of thousands of columns) and (eventually) on-device decompression on GPUs.

### Major features (per the project README, 2024-10)

- **Logical Types** — schema separate from physical layout.
- **Zero-Copy to Arrow** — canonicalised Vortex arrays convert to/from Arrow with no copy.
- **Extensible Encodings** — FastLanes, ALP, FSST, etc., as pluggable extensions; the chosen set is intentionally small and data-parallel for vectorised / GPU decoding.
- **Cascading Compression** — recursive nesting of encodings.
- **Pluggable Compression Strategies** — built-in `Compressor` based on BtrBlocks; substitutable.
- **Compute** — basic kernels operating directly on encoded data (e.g., filter pushdown).
- **Statistics** — per-array lazy summary stats, usable by compressor and planner.
- **Serialization** — zero-copy IPC + on-disk serialisation.
- **Columnar File Format** — in progress; the stretch goal is a Parquet-successor file format on top of the Vortex serde layer.
