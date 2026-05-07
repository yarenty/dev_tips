---
title: "Rust I/O — formats, parsers, byte buffers, big-data filesystems"
keywords: [rust, io, json, parsers, bytes, hdfs, excel, storage, serde]
status: reviewed
---

# Rust I/O — formats, parsers, byte buffers, big-data filesystems

Articles in this section cover the **data-shape side of Rust I/O**: turning bytes into typed values and back. Network I/O lives in [`../net/`](../net/README.md); web-shaped I/O in [`../web/`](../web/README.md); database I/O under [`db/`](../../../db/README.md). The split below is by *what kind of data* you're moving, not by *transport*.

## Articles

- [[bytes_manipulation|bytemuck — safe bit-cast between POD types]] — typed reinterpret of raw `&[u8]` buffers; the safe-Rust answer to `transmute`.
- [[json|JSON in Rust (serde_json, simd-json, sonic-rs, Arrow)]] — when to pick which JSON parser; the serde-default vs. SIMD-perf split.
- [[parsers|Parsers in Rust (pest, nom, chumsky, winnow, combine)]] — PEG vs. combinators; when to reach for which.
- [[xls|Excel / spreadsheets in Rust (calamine + writers)]] — read-only calamine + the rust_xlsxwriter / umya-spreadsheet write story.
- [[hdfs|HDFS clients in Rust (libhdfs3, hdfs-native)]] — legacy big-data filesystem niche; modern alternatives (object stores, OpenDAL) noted.

## Subsections

- [[storage/README|Storage]] — landing page for object-store / lakehouse-format Rust crates; currently a stub pointing at OpenDAL / `object_store` upstream.

## Decision shortcut

| You have… | Reach for |
|-----------|-----------|
| A `&[u8]` from a wire / mmap / GPU buffer | `bytemuck` (this section) |
| JSON text → typed structs | `serde_json` (this section) |
| JSON text → analytics columns | `arrow-json` (this section) |
| A custom DSL / config grammar | `pest` (this section) |
| A binary wire format | `nom` / `winnow` (this section) |
| A real programming language to parse | `chumsky` + `logos` (this section) |
| `xlsx` to read | `calamine` (this section) |
| `xlsx` to write | `rust_xlsxwriter` |
| HDFS access | `hdfs-native` or `object_store + hdfs-object-store` (this section) |
| S3 / GCS / Azure | `object_store` or OpenDAL (`storage/`) |

## See also

- [[../net/README|Rust net]] — sister section: network protocols (gRPC / MQTT / NATS / tunnels).
- [[../web/README|Rust web]] — HTTP-shaped I/O (axum / actix / reqwest).
- [[../concurrency/README|Rust concurrency]] — `tokio`, async-stream, `par-stream`; the runtime layer that I/O lives on.
- [[../tooling/README|Rust tooling]] — workflow tools that often process file formats.
- [[db/formats/README|db/formats]] — Parquet / ORC / Iceberg / Avro; columnar formats commonly read with Arrow.
- [[messaging/README|Messaging]] — the substrate side (Kafka / NATS / MQTT brokers).

## Keywords

`#rust` `#io` `#json` `#parsers` `#serde` `#bytes` `#hdfs` `#excel`
