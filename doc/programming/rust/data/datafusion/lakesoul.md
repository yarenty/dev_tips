---
title: LakeSoul — unified streaming + batch lakehouse format
main_link: https://github.com/lakesoul-io/LakeSoul
keywords: [lakesoul, lakehouse, streaming, batch, datafusion, spark, flink, acid, china]
status: reviewed
---

# LakeSoul — unified streaming + batch lakehouse format

**Main link:** <https://github.com/lakesoul-io/LakeSoul>

## Summary

**LakeSoul** is a cloud-native lakehouse framework (originally from DMetaSoul, China) that pitches itself as a unified streaming-and-batch table format with scalable metadata management, ACID transactions, efficient upserts, schema evolution, and unified streaming + batch processing. The compute story is multi-engine — Spark, Flink, Presto, PyTorch — and storage runs on HDFS or S3. The native I/O layer is written in Rust (the `lakesoul-io` crate) and integrates with DataFusion; the catalog and writer paths run on the JVM.

## Insight

LakeSoul slots into the **open table format** category alongside [[delta|Delta Lake]], [[iceberg|Apache Iceberg]], and Apache Hudi — but it's the least famous of the four, with mostly Chinese-language documentation and a much smaller English-speaking community. The honest framing for English-language readers in 2024-2025 is **watch, don't adopt**: the technical pitch (streaming + batch unification, AI-aware MPP support, PyTorch loader) is interesting and partly differentiated, but production references outside China are scarce and the lakehouse-format space is consolidating around Iceberg + Delta. Reach for it only if you already have a Chinese-ecosystem dependency or a specific feature gap that nothing else fills.

The differentiating bullet points worth knowing about:

- **Native (Rust) I/O layer** — the `lakesoul-io` crate uses Apache Arrow + DataFusion under the hood, which makes it cheaper than Delta/Iceberg to embed in non-JVM Rust services (in principle).
- **MPP + AI compute mode** — explicit support for PyTorch dataloaders alongside Spark/Flink/Presto SQL, framed as a lakehouse for ML training data.
- **Upsert-first design** — like Hudi, optimised for record-level upserts rather than overwrite-the-partition.
- **CDC-style streaming ingest** — primary use case in DMetaSoul deployments.

The competitive landscape is unforgiving: Iceberg has the multi-engine catalog story, Delta has Databricks' marketing weight, Hudi has the streaming/upsert lineage, and LakeSoul is competing with all three on overlapping axes with a fraction of the engineering bandwidth.

## Similar / related topics

- [[delta]] — Delta Lake / `delta-rs`, the Databricks-lineage table format.
- [[iceberg]] — Apache Iceberg + `iceberg-rust`, the broadest-adoption table format.
- **Apache Hudi** — the streaming/upsert-first table format LakeSoul most resembles.
- [[vortex]] — file-format layer (one level below table format).
- [[../README|rust/data]] — Rust data landing.

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[delta]] — Delta Lake comparison.
- [[iceberg]] — Iceberg comparison.
- [[../../../../db/formats/README|db/formats]] — broader format landing.

## Keywords

`#lakesoul` `#lakehouse` `#streaming` `#batch` `#datafusion` `#spark` `#flink` `#acid` `#china`

## References / raw notes

- Repo: <https://github.com/lakesoul-io/LakeSoul>
- Project background (in part): DMetaSoul (Chinese-ecosystem big-data vendor).

> LakeSoul is a cloud-native Lakehouse framework that supports scalable metadata management, ACID transactions, efficient and flexible upsert operation, schema evolution, and unified streaming & batch processing.
>
> LakeSoul supports multiple computing engines to read and write lake warehouse table data, including Spark, Flink, Presto, and PyTorch, and supports multiple computing modes such as batch, stream, MPP, and AI. LakeSoul supports storage systems such as HDFS and S3.
