---
title: Hive UDFs (and the JVM-bridge angle)
main_link: https://cwiki.apache.org/confluence/display/Hive/HivePlugins
keywords: [hive, udf, jvm, jni, hadoop, rust]
status: reviewed
review_date: 2026/05/03
---

# Hive UDFs (and the JVM-bridge angle)

**Main link:** <https://cwiki.apache.org/confluence/display/Hive/HivePlugins>

## Summary

Apache Hive's UDF mechanism is one of the oldest "user-defined functions" stories in the big-data world: write a Java class extending `UDF` (simple) or `GenericUDF` (typed; supports `INSERT … SELECT`-shaped operators), package it in a JAR, `ADD JAR` it into the Hive session, then `CREATE FUNCTION foo AS 'com.example.MyUDF'`. Filed in this section as **the canonical "JVM-bridge UDF" reference point** — there is no native Rust Hive UDF SDK, so reaching Hive from Rust means going through the JVM via JNI, building a `.so` that Java loads, and exposing wrapper Java classes that delegate into your Rust code.

## Insight

The reason this article lives in the Rust SQL-engine section at all is the **bridge pattern**: it is the same problem you face when writing [[spark|Spark UDFs in Rust]], and the same one [[../data/datafusion/blaze|Blaze]] / [[../data/datafusion/gluten|Gluten]] solve at the engine level. The minimum viable path is `jni-rs` (see [[../interop/to_java]]) → expose `extern "system"` functions matching `Java_…` JNI naming → compile to a shared library → load it from a tiny `GenericUDF` wrapper that calls into Rust via `System.loadLibrary` + `native` methods. That works but is unergonomic; if you find yourself doing this seriously, the smart move is to migrate the workload to a Rust-native engine (DataFusion, Polars) and skip Hive. **Don't** start a new Hive integration in 2025 — Hive is a legacy system; modern equivalents are: Trino / Presto for the SQL-on-anything role, Spark for the JVM batch role, Iceberg / Delta for the table format role. Use this article as a pointer for understanding the JVM-bridge cost and as a comparison anchor when reading about Spark UDFs.

## Similar / related topics

- [[spark]] — the modern equivalent; same JVM-bridge cost, much more active ecosystem.
- [[../interop/to_java]] — the canonical "I have a Rust crate, I want to call it from JVM" recipe.
- **Apache Calcite** — the parser/planner library used by lots of JVM-side SQL engines (Hive, Drill, Flink); the JVM-world counterpart of [[../data/sqlparser|sqlparser-rs]].
- **Hive UDAFs / UDTFs** — aggregate and tabular variants of the same pattern.
- [[../../../db/udf/external_udfs|external UDFs (cross-engine survey)]] — wider context.

## Internal links

- [[README]]
- [[spark]] — Spark UDF article, same bridge cost.
- [[../interop/to_java]] — `jni-rs` how-to.
- [[../data/datafusion/blaze|Blaze]] / [[../data/datafusion/gluten|Gluten]] — modern alternative: replace JVM execution with native (DataFusion/Velox) under Spark/Hive plans.
- [[../../../db/udf/external_udfs|external UDFs (db/udf survey)]]

## Keywords

`#hive` `#udf` `#jvm` `#jni` `#hadoop` `#rust-bridge`

## References / raw notes

- Hive `CREATE FUNCTION` reference: <https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL>
- HivePlugins (UDF / UDAF / UDTF): <https://cwiki.apache.org/confluence/display/Hive/HivePlugins>
- Older tutorials originally linked here:
  - <https://data-flair.training/blogs/hive-udf/>
  - <https://www.guru99.com/hive-user-defined-functions.html>
- See also Spark's documentation on integrating Hive UDFs: <https://spark.apache.org/docs/latest/sql-ref-functions-udf-hive.html>
