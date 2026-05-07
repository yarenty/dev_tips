---
title: "JSON in Rust (serde_json, simd-json, sonic-rs, Arrow JSON)"
main_link: https://github.com/serde-rs/json
keywords: [rust, json, serde, simd-json, sonic-rs, arrow, performance]
status: reviewed
---

# JSON in Rust (serde_json, simd-json, sonic-rs, Arrow JSON)

**Main link:** <https://github.com/serde-rs/json>

## Summary

Rust's JSON story is split between **`serde_json`** (the de-facto default — typed (de)serialization on top of the `serde` framework) and a small family of performance-focused alternatives — **`simd-json`**, **`sonic-rs`**, and the columnar **Arrow JSON decoder** in DataFusion / arrow-rs. Pick `serde_json` for almost everything; reach for one of the others when JSON parsing actually shows up as a bottleneck (typically wide-row analytics, log ingestion, or stream processing of millions of records/sec).

## Insight

The decision tree is short:

- **`serde_json`** — default. Universal, integrates with every other serde-shaped crate, deserializes straight into your structs via `#[derive(Deserialize)]`. Cost: ~150-300 MB/s on cold parsing, more if your structs are simple.
- **`simd-json`** — port of Daniel Lemire's [simdjson](https://simdjson.org/) C++ library by Heinz Gies. SIMD-accelerated, uses a "tape" representation internally. Drop-in for `serde_json` via `simd_json::serde::from_slice`. Wins big on **DOM-style** parsing of multi-megabyte payloads; the win shrinks for small structs.
- **`sonic-rs`** — ByteDance's port of their C++ Sonic-CPP. Often *faster than* simd-json on x86-64 with AVX2. Smaller ecosystem, fewer eyeballs; treat as the speed champion for compute-bound ingestion paths where you've measured.
- **Arrow JSON decoder** ([arrow-rs](https://arrow.apache.org/rust/arrow_json/)) — fundamentally different shape: parses JSON straight into Arrow `RecordBatch`es (columnar). The right answer when JSON is just a wire format en route to a columnar engine (DataFusion, Polars, ClickHouse-style aggregations). [Arroyo's blog post](https://www.arroyo.dev/blog/fast-arrow-json-decoding) is a good walk-through of why this beats row-at-a-time decoding for analytics.

When JSON is genuinely your bottleneck and the schema is fixed, also consider:

- **Skipping JSON.** If both ends are Rust, `bincode` / `postcard` / `rkyv` / `bitcode` are 10-100× faster.
- **MessagePack / CBOR / FlatBuffers / Protobuf.** Binary cousins of JSON, all with serde integrations.
- **`orjson`** — note this is the Python library (Rust-implemented); on the Rust side, that's `serde_json`/`simd-json` already.

Gotchas:

1. **Numeric precision.** JSON's "number" is one type; serde_json's `Value::Number` round-trips f64 by default. For arbitrary-precision (BigDecimal, blockchain), enable the `arbitrary_precision` feature.
2. **`#[serde(untagged)]` is slow.** It's a sequential try-each-variant; for hot paths use a discriminator field with `#[serde(tag = "type")]`.
3. **Borrowing strings.** `&'a str` deserialization avoids allocation but only works when the source survives — not for streaming / `Read`-based input.
4. **`simd-json` mutates the input buffer.** It needs `&mut [u8]` because the SIMD parser writes into the source buffer. Check this if you're passing read-only data.

## Similar / related topics

- [serde](https://serde.rs/) — the framework underneath all of the above; the canonical Rust (de)serialization story.
- [`bincode`](https://crates.io/crates/bincode) / [`postcard`](https://crates.io/crates/postcard) — binary serde formats; pick when you control both ends.
- [`rkyv`](https://crates.io/crates/rkyv) — zero-copy archive format, ~free deserialization.
- [Apache Arrow JSON decoder](https://arrow.apache.org/rust/arrow_json/) — columnar parser for analytics pipelines.
- [transform.tools JSON-to-Rust](https://transform.tools/json-to-rust-serde) — paste sample JSON, get a serde struct skeleton.

## Internal links

<!-- reviewed -->

- [[programming/rust/io/README|Rust I/O]] — section landing page.
- [[programming/rust/io/parsers|parsers]] — when JSON isn't enough and you need a real grammar.
- [[programming/rust/io/bytes_manipulation|bytemuck]] — sibling article for fixed-layout binary, the opposite end of the (de)serialization space.

## Keywords

`#rust` `#json` `#serde` `#simd-json` `#sonic-rs` `#arrow` `#performance`

## References / raw notes

### serde_json (the default)

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

### Schema-from-sample helper

[transform.tools JSON to serde](https://transform.tools/json-to-rust-serde) — paste real JSON, get a `#[derive(Serialize, Deserialize)]` Rust struct.

### Fast JSON paths (when you've measured)

- [`simd-json`](https://github.com/simd-lite/simd-json) — Rust port of simdjson; drop-in for `serde_json::from_slice`.
- [`sonic-rs`](https://github.com/cloudwego/sonic-rs) — ByteDance's high-perf JSON; often beats simd-json on AVX2.
- [Fast Arrow JSON decoding](https://www.arroyo.dev/blog/fast-arrow-json-decoding) — Arroyo's write-up on columnar JSON parsing into Arrow `RecordBatch`es. The right mental model when JSON is en route to a columnar engine (DataFusion, Polars).

### Decision shortcut

| Workload | Pick |
|----------|------|
| Most app code | `serde_json` |
| Log ingestion / multi-MB DOM | `simd-json` |
| You've benchmarked & sonic wins | `sonic-rs` |
| JSON → analytics columns | Arrow JSON |
| Binary, both ends Rust | `bincode` / `postcard` / `rkyv` |
