---
title: "HDFS clients in Rust (libhdfs3, hdfs-native)"
main_link: https://github.com/apache/hawq/tree/master/depends/libhdfs3
keywords: [rust, hdfs, hadoop, libhdfs3, hdfs-native, jni, big-data, opendal, object-store]
status: reviewed
---

# HDFS clients in Rust (libhdfs3, hdfs-native)

**Main link:** <https://github.com/apache/hawq/tree/master/depends/libhdfs3>

## Summary

[**libhdfs3**](https://github.com/apache/hawq/tree/master/depends/libhdfs3) is a native C++ client for HDFS that re-implements the Hadoop RPC and DataNode transfer protocols directly — no JVM, no JNI, no copy of the Hadoop jars on every host. It originated inside Pivotal HAWQ and lives today in Apache HAWQ. From Rust you call it through a thin FFI wrapper. As of 2024, [**hdfs-native**](https://crates.io/crates/hdfs-native) is the modern pure-Rust contender (Kerberos, HA, encryption, growing maturity), and the JNI-based [`hdfs`](https://crates.io/crates/hdfs) and [`fs-hdfs`](https://crates.io/crates/fs-hdfs) crates remain the safe-but-heavy fallback.

## Insight

Reach for an HDFS client only when **HDFS is non-negotiable** — i.e. you're talking to an existing on-prem Hadoop cluster, a CDP/CDH deployment, or a vendor product (Druid, Impala, HBase) that pins HDFS as its storage layer. New systems essentially never start with HDFS today: object-store APIs (S3, GCS, Azure Blob, MinIO/Ceph for on-prem) have eaten the use case, and the Rust analytics ecosystem has standardised on [`object_store`](https://crates.io/crates/object_store) and [Apache OpenDAL](https://opendal.apache.org/) as the abstraction layer. If you can write to S3-compat instead, do that.

When HDFS *is* the requirement, the Rust picker is roughly:

- **libhdfs3 (FFI)** — most mature, no JVM. Ships its own Kerberos and HA story. Build complexity (CMake, libxml2, Protobuf) is the cost.
- **hdfs-native (pure Rust)** — newer, growing fast, no C deps. The right choice for new projects unless you hit a missing feature (some advanced ACL / EC / federation paths).
- **hdfs / fs-hdfs (JNI)** — drag the whole JVM in. Operational nightmare for anything that wants to be a single binary; only pick if a feature gap forces it.

A common pattern in Rust analytics engines (DataFusion, delta-rs, Ballista) is to plug HDFS into [`object_store`](https://crates.io/crates/object_store) via [`hdfs-object-store`](https://crates.io/crates/hdfs-object-store) (built on hdfs-native) so the rest of the engine stays HDFS-agnostic.

Gotchas:

1. **Kerberos auth.** If your cluster uses Kerberos (most do), you need a working `krb5.conf` + a keytab + the right principal at runtime. Easy to mis-configure; test with `klist` before debugging the Rust side.
2. **HA name-services.** Hadoop HA uses logical names like `nn-cluster` resolved through `hdfs-site.xml`. All clients need that XML available (or its key fields baked into config) — otherwise the client tries to connect to a nameservice and fails.
3. **Short-circuit reads / RDMA.** libhdfs3 supports them; pure-Rust clients usually do not. For dense ETL workloads on co-located storage that gap can matter.

## Similar / related topics

- [`object_store`](https://crates.io/crates/object_store) — Apache Arrow's unified object-store crate (S3 / GCS / Azure / local / HTTP); the modern default.
- [Apache OpenDAL](https://opendal.apache.org/) — broader cross-language data-access layer with HDFS, S3, IPFS, etc. backends.
- [hdfs-native](https://crates.io/crates/hdfs-native) — pure-Rust HDFS client, the modern choice.
- [`fs-hdfs`](https://crates.io/crates/fs-hdfs) / [`hdfs`](https://crates.io/crates/hdfs) — JNI-based Rust crates; safe but require the JVM at runtime.
- [Apache Ozone](https://ozone.apache.org/) — Hadoop project's S3-compatible object store, the official "post-HDFS" path.

## Internal links

<!-- reviewed -->

- [[programming/rust/io/README|Rust I/O]] — section landing page.
- [[programming/rust/io/storage/README|storage]] — sibling section for object-store / OpenDAL / lakehouse formats.
- [[db/formats/README|db/formats]] — Parquet / ORC / Iceberg formats commonly stored on HDFS or S3.

## Keywords

`#rust` `#hdfs` `#hadoop` `#libhdfs3` `#hdfs-native` `#big-data` `#object-store` `#opendal`

## References / raw notes

### libhdfs3

> Libhdfs3, designed as an alternative implementation of libhdfs, is implemented based on native Hadoop RPC protocol and HDFS data transfer protocol. It gets rid of the drawbacks of JNI, and it has a lightweight, small memory-footprint code base. In addition, it is easy to use and deploy.

The Hadoop Distributed File System (HDFS) is a distributed file system designed to run on commodity hardware. HDFS is highly fault-tolerant and is designed to be deployed on low-cost hardware. HDFS provides high-throughput access to application data and is suitable for applications that have large data sets.

HDFS is implemented in Java and additionally provides a JNI-based C library, *libhdfs*. To use libhdfs, users must deploy the HDFS jars on every machine — operational complexity for non-Java clients that just want to integrate with HDFS.

**Comparison of Rust HDFS crates (as of 2024):**

| Crate | Approach | Status |
|-------|----------|--------|
| [`hdfs`](https://crates.io/crates/hdfs) | JNI wrapper | mature but JVM-bound |
| [`fs-hdfs`](https://crates.io/crates/fs-hdfs) | JNI wrapper | mature but JVM-bound |
| [`hdfs-native`](https://crates.io/crates/hdfs-native) | pure Rust | newer, increasingly viable |
| `libhdfs3` (via FFI) | C++ native | mature, no JVM, CMake build |

### Migration / modernisation pointers

- [`object_store`](https://crates.io/crates/object_store) — the abstraction most Rust analytics code uses; HDFS plugs in via [`hdfs-object-store`](https://crates.io/crates/hdfs-object-store).
- [Apache OpenDAL](https://opendal.apache.org/) — broader, cross-language data-access layer; `opendal::services::Hdfs` covers HDFS via libhdfs.
- [Apache Ozone](https://ozone.apache.org/) — Hadoop's S3-compatible successor object store.

---

### Misplaced content (originally collected here, will be relocated)

The original raw-notes section also contained capsule write-ups for **rdkafka, DataFusion, Arrow, JNI, mimalloc, async-stream, futures-util, object_store, PyO3** — they belong under their respective sections (`messaging/`, `db/distributed/datafusion/`, `programming/rust/concurrency/`, `programming/rust/interop/`, etc.) and most are already covered there as part of P5.M / P5.T / P5.W / P5.Y. Not duplicated here to keep this article focused on HDFS.
