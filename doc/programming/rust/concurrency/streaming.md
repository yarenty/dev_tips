---
title: "par-stream — async streams with parallelism"
main_link: https://github.com/jerry73204/par-stream
keywords: [par-stream, rust, async, streams, parallelism, futures, tokio]
status: reviewed
---

# par-stream — async streams with parallelism

**Main link:** <https://github.com/jerry73204/par-stream>

## Summary

[`par-stream`](https://github.com/jerry73204/par-stream) is a small Rust library that adds **parallel combinators to async streams** — `par_map`, `par_then`, `par_for_each`, `par_batching`, `scatter` / `gather`, `par_sort_unstable`. It plugs into the `futures::Stream` trait so anything that yields a `Stream` (a Tokio channel, a paginated HTTP fetcher, a database cursor) can be consumed by N concurrent worker tasks instead of one. The result is *ordered* (`par_map`) or *unordered* (`par_map_unordered`) downstream, your choice.

## Insight

Reach for `par-stream` when the producer is async and **per-item work is also async** but slow enough that you want a worker pool. The canonical shape is "fetch then process": one upstream stream of URLs / S3 keys / job IDs, each item triggers an `async fn process(item)` that does I/O, and you want N of them in flight at once. That's awkward with raw `futures::stream::StreamExt::buffer_unordered` once you also need batching, scatter/gather, or per-stage parallelism — `par-stream` gives you the combinator names directly.

How it slots vs. the obvious neighbours:

- **vs. [`futures::stream::buffer_unordered(n)`](https://docs.rs/futures/latest/futures/stream/trait.StreamExt.html#method.buffer_unordered)** — the stdlib-of-async equivalent. Covers the simple case of "run N futures in parallel"; `par-stream` adds richer dataflow (scatter/gather, batching, parallel sort) and ordered variants.
- **vs. [`tokio_stream`](https://docs.rs/tokio-stream/)** — `tokio_stream` is the adapters between Tokio types (channels, intervals) and `Stream`; it does not add parallelism. They compose: use `tokio_stream` to *get* a `Stream`, use `par-stream` to *parallelise* it.
- **vs. [`rayon`](https://crates.io/crates/rayon)** — Rayon parallelises *sync, CPU-bound* iterators across a thread pool. par-stream parallelises *async, I/O-bound* streams across Tokio tasks. Different runtime, different shape — pick on whether your per-item work is `async fn` or `fn`.
- **vs. handcrafted `mpsc` + `tokio::spawn` workers** — that's effectively what par-stream is doing under the hood. The library is a few hundred lines; if your pipeline is one stage you can absolutely write it yourself, and many people do. The library earns its keep when you have *several* parallel stages chained together.

Gotchas:

1. **Maintenance is light.** It's a small, single-author crate that hasn't seen rapid releases; treat it as "stable utility code" rather than a moving target.
2. **Ordered combinators have to buffer.** `par_map` (ordered) holds back later items until earlier ones complete, just like `buffered`; this can balloon memory if one item is much slower than the rest. Use `par_map_unordered` when you don't care about order.
3. **You're still on a Tokio runtime.** par-stream doesn't bring its own executor — it spawns onto the ambient runtime. Holding a sync `Mutex` across an `.await` inside one of these workers will block the runtime worker just like any other Tokio task.

## Similar / related topics

- [`futures::stream::StreamExt`](https://docs.rs/futures/latest/futures/stream/trait.StreamExt.html) — `buffered` / `buffer_unordered` / `for_each_concurrent`; the lowest-common-denominator way to get parallel async streams.
- [`tokio_stream`](https://docs.rs/tokio-stream/) — adapters from Tokio primitives (channels, intervals, `ReceiverStream`) into `Stream`; pairs with par-stream.
- [`rayon`](https://crates.io/crates/rayon) — sync data-parallel iterators; the right tool when work is CPU-bound rather than I/O-bound.
- [[timely]] — heavyweight dataflow framework for when you outgrow combinator-style streams and need explicit operators, timestamps, and feedback loops.
- [[tokio]] — the underlying runtime; par-stream is a layer on top.

## Internal links

<!-- reviewed -->

- [[tokio]] — runtime par-stream's workers spawn onto.
- [[timely]] — heavyweight alternative when streams + combinators stop being enough.
- [[concurrency/README|Rust concurrency]] — landing page; par-stream lives in the dataflow / streaming row.

## Keywords

`#rust` `#async` `#streams` `#parallelism` `#par-stream` `#futures` `#tokio`

## References / raw notes

### What it is

> par-stream is an asynchronous parallel stream processing library for Rust.

Source / examples: <https://github.com/jerry73204/par-stream/blob/master/src/par_stream.rs>

### Combinators (from the README)

- Ordered parallel processing dataflow (`par_map`)
- Unordered parallel processing dataflow (`par_map_unordered`)
- Scatter and gather
- Parallel merge-sort
- Parallel shuffle
- `par_batching` for grouping items into batches before downstream work

### Real-world example

A good production-shape consumer is the [Lychee link checker](https://github.com/lycheeverse/lychee), specifically its collector:

<https://github.com/lycheeverse/lychee/blob/master/lychee-lib/src/collector.rs>

It uses parallel streams to fan out URL discovery and checking across many concurrent HTTP requests.

### Useful entry points

- Repo: <https://github.com/jerry73204/par-stream>
- `futures::stream::buffer_unordered` — lighter-weight alternative for the simple case: <https://docs.rs/futures/latest/futures/stream/trait.StreamExt.html#method.buffer_unordered>
- `tokio_stream` — for getting a `Stream` out of Tokio primitives in the first place: <https://docs.rs/tokio-stream/>
