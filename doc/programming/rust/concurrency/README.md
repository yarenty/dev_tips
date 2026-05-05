---
title: Rust concurrency
keywords: [concurrency, rust, async, threads, channels]
status: reviewed
---

# Rust concurrency

Rust has two large, mostly-disjoint concurrency stories — the **sync world** (OS threads + channels + locks, what you write when you `std::thread::spawn`) and the **async world** (cooperatively-scheduled `async fn` driven by an executor like Tokio). Picking the right one matters more than picking specific crates inside either; see [[tokio]] vs [[crossbeam]] for the head-to-head.

## Articles

- [[tokio]] — the canonical async runtime; what to reach for when you have many concurrent I/O-bound tasks.
- [[crossbeam]] — channels, scoped threads, lock-free queues for the sync world.
- [[actors]] — Actix-style actor framework.
- [[salsa]] — incremental, on-demand, parallel computation framework (also used inside `rust-analyzer`).
- [[streaming]] — `par-stream` — async streams with parallelism.
- [[timely]] — Rust port of [Timely Dataflow](https://timelydataflow.github.io/timely-dataflow/) for high-throughput data-parallel computation.

## When to pick what

| Workload | Reach for |
|----------|-----------|
| 10k+ concurrent network connections | [[tokio]] |
| Pipeline of long-running threads passing messages | [[crossbeam]] |
| Embarrassingly parallel CPU work over a slice | [`rayon`](https://crates.io/crates/rayon) (`par_iter()`) |
| Dataflow / streaming / windowed aggregations | [[timely]], [[streaming]] |
| "I have an actor model in mind" | [[actors]] |
| Incremental compilation-style memoisation | [[salsa]] |

## Keywords

`#rust` `#concurrency` `#async` `#threads` `#channels` `#tokio` `#crossbeam`
