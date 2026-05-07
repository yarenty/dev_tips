---
title: Blaze — Spark accelerator on DataFusion
main_link: https://github.com/kwai/blaze
keywords: [blaze, spark, datafusion, native, vectorized, accelerator, kuaishou, arrow]
status: reviewed
---

# Blaze — Spark accelerator on DataFusion

**Main link:** <https://github.com/kwai/blaze>

## Summary

**Blaze** is a Spark plugin (originally open-sourced by Kuaishou / Kwai) that takes a fully optimised Spark physical plan and rewrites it into a DataFusion execution plan, then runs the compute-intensive operators inside Spark executors as native Rust code via JNI. The data path is Apache Arrow throughout, so Blaze returns `ArrowColumnarBatch` to the JVM with no row materialisation. The pitch is the usual columnar / vectorised / native trifecta — Spark's distributed control plane + DataFusion's single-node speed — sold mainly through TPC-DS benchmark numbers.

## Insight

Blaze sits in the **"native Spark accelerator"** category that has become crowded since 2022. The nearest neighbours are:

| Project | Vendor | Native engine | Status |
|---|---|---|---|
| **Blaze** | Kuaishou (independent) | DataFusion (Rust) | Active, no Apache governance |
| **Apache Gluten** | Intel + community → Apache Incubator | Velox or ClickHouse (C++) | Most "official" of the bunch |
| **Apache Comet** | Apple → Apache Incubator | DataFusion (Rust) | Newer, the more idiomatic DataFusion path |
| **Photon** | Databricks | C++, proprietary | Closed source, only on Databricks runtimes |
| **Velox** | Meta | — (it's the engine) | A library, not a Spark plugin |

For a Rust-on-DataFusion accelerator the modern recommendation is **Comet** (Apache governance, Apple-backed, larger contributor base); Blaze remains the older, single-vendor incumbent on the same architectural slot. The mental model is identical to Comet: ScalaSpark plan → Substrait/protobuf → DataFusion plan → native execution → Arrow back to the JVM.

Gotchas: any Spark plugin lives or dies by **operator coverage** — anything unsupported falls back to vanilla Spark with a `RowToColumnar`/`ColumnarToRow` round-trip that can erase the speed-up; the JNI / off-heap memory dance interacts with `spark.memory.offHeap.size`; and you're locking your cluster to whichever Spark minor versions the project's shim layer supports.

## Similar / related topics

- [[gluten]] — Apache Gluten, the Velox/ClickHouse-backed sibling.
- **Apache Comet** — the more "official" DataFusion-backed Spark plugin (Apache Incubator).
- **Photon** — Databricks' proprietary C++ accelerator with the same pitch.
- **Velox (Meta)** — the C++ vectorised engine Gluten and Presto-on-Velox use.
- [[rapids]] — NVIDIA's GPU-side Spark accelerator (different axis: GPU vs CPU-native).

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[gluten]] — sibling Spark accelerator (Velox / ClickHouse backends).
- [[rapids]] — GPU-side Spark accelerator.
- [[../README|rust/data]] — Rust data landing.
- [[../../../../db/analytics/datafusion/README|db/analytics/datafusion]] — DataFusion analytics-side notes.

## Keywords

`#blaze` `#spark` `#datafusion` `#native` `#vectorized` `#accelerator` `#kuaishou` `#arrow`

## References / raw notes

- Repo: <https://github.com/kwai/blaze>
- Apache Comet (the modern DataFusion-on-Spark sibling): <https://datafusion.apache.org/comet/>

### Architecture, per the project README

> Blaze takes a fully optimized physical plan from Spark, mapping it into DataFusion's execution plan, and performs native plan computation in Spark executors.

High-level components:

- **Spark Extension** — hooks the accelerator into Spark's execution lifetime.
- **Spark Shims** — version-specific glue per Spark minor.
- **Native Engine** (Rust) — `ExecutionPlan` protobuf spec, JNI gateway, custom operators / expressions / functions.

Extension axes (inherited from DataFusion's pluggability):

- Object stores
- Operators
- Scalar and aggregate functions
- File formats

The maintainers' guidance is to extend DataFusion upstream first, then wire the new capability into Blaze with small `plan-serde` + extension-translation tweaks.
