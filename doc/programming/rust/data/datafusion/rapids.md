---
title: spark-rapids — NVIDIA GPU accelerator for Apache Spark
main_link: https://github.com/NVIDIA/spark-rapids
keywords: [rapids, spark-rapids, nvidia, gpu, cudf, accelerator, spark, columnar]
status: reviewed
---

# spark-rapids — NVIDIA GPU accelerator for Apache Spark

**Main link:** <https://github.com/NVIDIA/spark-rapids>

## Summary

**spark-rapids** is NVIDIA's plugin that accelerates Apache Spark SQL and DataFrame operations on GPUs by replacing JVM-side execution with **cuDF** (the GPU columnar dataframe library from the broader [RAPIDS](https://rapids.ai/) ecosystem). It hooks into Spark's columnar API, transparently offloads supported operators to the GPU, and falls back to CPU for the rest. The link in the references points specifically at the **UDF compiler** subproject — a Scala-bytecode → cuDF-IR translator that lets some plain Scala UDFs run on the GPU without a hand-written CUDA rewrite.

## Insight

This is the **GPU axis** of the Spark-acceleration landscape. The CPU-native plugins ([[blaze|Blaze]], [[gluten|Gluten]], Apache Comet) all swap the JVM for a vectorised C++/Rust engine on the same hardware; spark-rapids instead swaps the *processor* — same JVM control plane, GPU compute. The two axes are independent and arguably complementary, though in practice you pick one.

When to reach for spark-rapids:

- you already pay for **GPU-equipped clusters** (Databricks GPU runtimes, EMR GPU instances, on-prem DGX) and want the SQL/ETL part of your pipeline to use them, not just the ML training part,
- your workload is **wide joins, aggregations, sorts, and string ops** at scale (the kind cuDF is best at),
- you can absorb the operator-coverage caveat (anything unsupported falls back to CPU and the GPU↔CPU transfer can erase the speed-up).

When *not* to:

- small data (< single-node) — GPU memory transfer overhead dominates,
- workloads dominated by Python UDFs that cuDF can't translate,
- cost-sensitive batch where CPU-native (Gluten/Comet) gets you 2-4× without changing the hardware bill.

The **UDF compiler** is the most interesting subproject for the curious: it walks Scala JVM bytecode of a UDF and emits an equivalent cuDF expression, so user code can also run on the GPU without manual rewrites — though only for a constrained subset of operations.

The relationship to the broader RAPIDS family: cuDF (GPU dataframes) ↔ cuML (GPU ML) ↔ cuGraph (GPU graphs) ↔ cuSpatial — spark-rapids is the Spark-on-cuDF integration. Inside the DataFusion ecosystem, the GPU story is much earlier (a few experimental DataFusion-on-GPU forks exist, none production), so spark-rapids remains the canonical GPU-on-Spark answer.

## Similar / related topics

- [[gluten]] — Apache Gluten, CPU-native Spark accelerator (Velox/ClickHouse).
- [[blaze]] — Blaze, CPU-native Spark accelerator (DataFusion).
- **Apache Comet** — Apple's CPU-native DataFusion-on-Spark (Apache Incubator).
- **NVIDIA RAPIDS / cuDF** — the broader GPU-columnar stack underneath.
- **Photon** — Databricks' proprietary CPU C++ accelerator.

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[gluten]] — sibling Spark accelerator (CPU-native).
- [[blaze]] — sibling Spark accelerator (CPU-native, DataFusion).
- [[../../ml/cuda|cuda]] — Rust-side CUDA notes.
- [[../README|rust/data]] — Rust data section.

## Keywords

`#rapids` `#spark-rapids` `#nvidia` `#gpu` `#cudf` `#accelerator` `#spark` `#columnar`

## References / raw notes

- Repo: <https://github.com/NVIDIA/spark-rapids>
- UDF compiler subproject (the original entry point in these notes): <https://github.com/NVIDIA/spark-rapids/blob/main/udf-compiler/README.md>
- RAPIDS umbrella: <https://rapids.ai/>
- cuDF: <https://github.com/rapidsai/cudf>
