---
title: Apache Gluten — Spark JVM-to-native offload (Velox / ClickHouse)
main_link: https://gluten.apache.org/
keywords: [gluten, spark, velox, clickhouse, substrait, native, offload, accelerator, apache]
status: reviewed
review_date: 2026/05/03
---

# Apache Gluten — Spark JVM-to-native offload (Velox / ClickHouse)

**Main link:** <https://gluten.apache.org/>

## Summary

**Apache Gluten** (incubating) is a "middle layer" Spark plugin that offloads Spark SQL execution from the JVM to a pluggable native engine — currently **Meta's Velox (C++)** or **ClickHouse**. The architecture: Spark plans the query, Gluten translates the physical plan to **Substrait** (the cross-engine plan IR), the Substrait plan crosses JNI into the chosen native backend, the backend executes vectorised on Apache Arrow data, and Arrow-shaped columnar batches are returned to Spark via the columnar API introduced in Spark 3.0. Originally an Intel project, Gluten was donated to the Apache Software Foundation incubator; the goal is to keep Spark's distributed control plane and shuffle while replacing only the compute hot loop.

## Insight

Gluten is the most "official" entry in the **Spark native-accelerator** category. It shares the slot with [[blaze|Blaze]] (DataFusion-on-Spark, Kuaishou) and Apache Comet (DataFusion-on-Spark, Apple) — but Gluten's distinguishing trait is its **multi-backend** design (Velox or ClickHouse) and its bet on **Substrait** as the plan IR, which gives it engine-portability that single-backend alternatives don't. Reach for it when:

- your data team already runs Spark at scale and you want a drop-in 2-4× speed-up without a rewrite,
- you've evaluated Velox separately and want it as a Spark backend (without writing the integration yourself),
- you can tolerate the supported-distro list (centos7/8 and ubuntu20.04/22.04 historically — check current docs).

How it relates to the rest of the Spark-acceleration landscape:

| Project | Backend | Plan IR | Lineage |
|---|---|---|---|
| **Apache Gluten** | Velox / ClickHouse (C++) | Substrait | Intel → Apache Incubator |
| **Apache Comet** | DataFusion (Rust) | Spark protobuf | Apple → Apache Incubator |
| [[blaze\|Blaze]] | DataFusion (Rust) | Spark protobuf | Kuaishou |
| **Photon** | Custom C++, closed | proprietary | Databricks-only |
| [[rapids\|spark-rapids]] | NVIDIA cuDF (GPU) | Spark API hooks | NVIDIA |

The **fallback mechanism** is the whole game — Gluten is a *partial* offload, so any unsupported operator triggers `ColumnarToRow` → vanilla Spark → `RowToColumnar`, and a single unsupported expression in a hot loop can erase the speed-up. Test on representative workloads, not just TPC-DS. Memory accounting is the other footgun: Gluten allocates off-heap (`spark.memory.offHeap.size` is mandatory), and the off-heap budget is shared between Gluten and any other off-heap users (e.g. Arrow-based connectors).

## Similar / related topics

- [[blaze]] — Kuaishou's DataFusion-backed Spark plugin (same architecture, different backend).
- **Apache Comet** — Apple's DataFusion-backed Spark plugin (Apache Incubator, the most idiomatic DataFusion path).
- **Velox (Meta)** — the C++ vectorised engine Gluten ships as one of its backends.
- **Substrait** — the cross-engine plan IR Gluten standardises on.
- [[rapids]] — NVIDIA's GPU-side Spark accelerator (different axis: GPU vs CPU-native).

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[blaze]] — sibling Spark accelerator, DataFusion backend.
- [[rapids]] — GPU-side Spark accelerator.
- [[../../../../db/analytics/datafusion/README|db/analytics/datafusion]] — DataFusion analytics-side notes.
- [[../README|rust/data]] — Rust data section.

## Keywords

`#gluten` `#spark` `#velox` `#clickhouse` `#substrait` `#native` `#offload` `#accelerator` `#apache`

## References / raw notes

- Project site: <https://gluten.apache.org/>
- Repo: <https://github.com/apache/incubator-gluten>
- Velox (the canonical native backend): <https://github.com/facebookincubator/velox/>
- Substrait (the plan IR): <https://substrait.io/>
- ClickHouse backend (Kyligence fork): <https://github.com/Kyligence/ClickHouse>

### Architecture, per the project README

> "Gluten" is Latin for glue. The main goal of Gluten project is to "glue" native libraries with SparkSQL.

What Gluten does:

- Transform Spark's whole-stage physical plan to a Substrait plan and send it to native.
- Offload performance-critical data processing to a native library.
- Define clear JNI interfaces for native backends.
- Switch available native backends easily (Velox / ClickHouse).
- Reuse Spark's distributed control flow.
- Manage data sharing between JVM and native (Arrow + columnar shuffle).

Key components:

- **Query Plan Conversion** — Spark physical plan → Substrait plan.
- **Unified Memory Management** — controls native memory allocation.
- **Columnar Shuffle** — shuffles Gluten columnar data; the shuffle service still reuses Spark's, but with a columnar-exchange operator.
- **Fallback Mechanism** — falls back to vanilla Spark for unsupported operators with `ColumnarToRow` / `RowToColumnar` (both implemented natively).
- **Metrics** — surfaced into the Spark UI.
- **Shim Layer** — currently supports Spark 3.2 / 3.3 / 3.4 (the project tracks the latest 2-3 Spark releases).

### Quick-start (released JAR)

```sh
spark-shell \
  --master yarn --deploy-mode client \
  --conf spark.plugins=io.glutenproject.GlutenPlugin \
  --conf spark.memory.offHeap.enabled=true \
  --conf spark.memory.offHeap.size=20g \
  --conf spark.shuffle.manager=org.apache.spark.shuffle.sort.ColumnarShuffleManager \
  --jars https://github.com/apache/incubator-gluten/releases/download/v1.0.0/gluten-velox-bundle-spark3.2_2.12-ubuntu_20.04_x86_64-1.0.0.jar
```

### Custom build

```sh
export gluten_jar=/PATH/TO/GLUTEN/backends-velox/target/<gluten-jar>
spark-shell \
  --master yarn --deploy-mode client \
  --conf spark.plugins=io.glutenproject.GlutenPlugin \
  --conf spark.memory.offHeap.enabled=true \
  --conf spark.memory.offHeap.size=20g \
  --conf spark.driver.extraClassPath=${gluten_jar} \
  --conf spark.executor.extraClassPath=${gluten_jar} \
  --conf spark.shuffle.manager=org.apache.spark.shuffle.sort.ColumnarShuffleManager
```

For Velox-backend builds see *Build with Velox*; for ClickHouse, *Build with ClickHouse Backend* in the Apache docs.
