---
title: "crossbeam: lock-free concurrency primitives for sync Rust"
main_link: https://github.com/crossbeam-rs/crossbeam
keywords: [crossbeam, rust, concurrency, channels, mpmc, scoped-threads, sync]
status: reviewed
---

# crossbeam: lock-free concurrency primitives for sync Rust

**Main link:** <https://github.com/crossbeam-rs/crossbeam>

Docs: <https://docs.rs/crossbeam>

## Summary

[`crossbeam`](https://github.com/crossbeam-rs/crossbeam) is a family of crates that fill in the gaps in `std`'s sync-world concurrency primitives. The pieces you'll actually reach for, in rough order of usefulness:

- **`crossbeam-channel`** — multi-producer, multi-consumer channels (`unbounded`, `bounded`, `tick`, `after`, `select!`). Drop-in replacement for `std::sync::mpsc` with a richer API and (until recently) much better performance.
- **`crossbeam::scope`** / **`std::thread::scope`** — scoped threads that can borrow non-`'static` data; let you spawn workers that read a `&[T]` without an `Arc` or `clone()`.
- **`crossbeam-queue`** — bounded SPSC / MPMC lock-free queues (`ArrayQueue`, `SegQueue`).
- **`crossbeam-deque`** — work-stealing deque, the building block under [`rayon`](https://crates.io/crates/rayon)'s thread pool.
- **`crossbeam-epoch`** — epoch-based memory reclamation, used to build lock-free data structures safely.

## Insight

Use crossbeam (not Tokio) when your problem is shaped like "a fixed handful of long-running threads passing messages". A market-data fanout, a parser pipeline, a worker pool consuming from a queue — none of these need 10k concurrent tasks, so the async runtime is just overhead and the crossbeam primitives map cleanly onto the actual structure.

Two specific things worth knowing:

1. **`crossbeam_channel::select!` does what `tokio::select!` does, in the sync world.** You can wait on N channels (plus optional timeouts via `tick` / `after`) and react to whichever fires first. This is the killer feature that pushes most people from `std::mpsc` to crossbeam.
2. **`std::thread::scope` (stable since Rust 1.63) covers a lot of what `crossbeam::scope` used to.** If all you need is "spawn threads that borrow local data", you may not need the dependency at all anymore. Reach for `crossbeam::scope` only if you specifically want its richer thread-handle API.

The performance gap vs. `std::sync::mpsc` has narrowed — the standard library now uses parts of crossbeam internally — but the API surface is still the reason to pull it in.

## Similar / related topics

- [[tokio]] — what you'd reach for instead if your workload is actually I/O-bound and async-shaped.
- [`rayon`](https://crates.io/crates/rayon) — built on crossbeam-deque; the right tool for *data-parallel* CPU-bound work (`par_iter()`).
- [`flume`](https://github.com/zesterer/flume) — single-crate channel library, often quoted as a leaner alternative.
- [`parking_lot`](https://github.com/Amanieu/parking_lot) — drop-in replacements for `std::sync::{Mutex, RwLock, Condvar}` that are faster and smaller.
- [[folbrecht_algo_trading_series]] — real codebase that picks crossbeam over Tokio on purpose.

## Internal links

<!-- reviewed -->

- [[tokio]] — async-world alternative
- [[folbrecht_algo_trading_series]] — example of choosing crossbeam over Tokio in production

## Keywords

`#rust` `#concurrency` `#crossbeam` `#channels` `#mpmc` `#scoped-threads` `#sync`

## References / raw notes

### Channels with `select!`

```rust
use crossbeam_channel::{unbounded, select, after};
use std::time::Duration;

let (s1, r1) = unbounded::<i32>();
let (s2, r2) = unbounded::<&str>();

std::thread::spawn(move || s1.send(1).unwrap());
std::thread::spawn(move || s2.send("hello").unwrap());

select! {
    recv(r1) -> msg     => println!("from r1: {msg:?}"),
    recv(r2) -> msg     => println!("from r2: {msg:?}"),
    recv(after(Duration::from_secs(1))) -> _ => println!("timeout"),
}
```

### Scoped threads (stable, no crossbeam needed)

```rust
let data = vec![1, 2, 3, 4];

std::thread::scope(|s| {
    s.spawn(|| {
        // can borrow `data` directly — no Arc, no clone
        println!("first half: {:?}", &data[..2]);
    });
    s.spawn(|| {
        println!("second half: {:?}", &data[2..]);
    });
});
// `data` still owned here; both threads are joined automatically
```

If you need the older crossbeam form (`crossbeam::scope(|s| { ... })`) it works the same way and additionally lets the spawned closure return a value via `s.spawn(...).join()`.

### Bounded MPMC queue

```rust
use crossbeam_queue::ArrayQueue;
use std::sync::Arc;

let q = Arc::new(ArrayQueue::<u32>::new(1024));

let producer = {
    let q = q.clone();
    std::thread::spawn(move || {
        for i in 0..1000 {
            // `push` returns Err(value) if the queue is full
            while q.push(i).is_err() { std::hint::spin_loop(); }
        }
    })
};

let consumer = {
    let q = q.clone();
    std::thread::spawn(move || {
        let mut count = 0;
        while count < 1000 {
            if let Some(_v) = q.pop() { count += 1; }
        }
    })
};

producer.join().unwrap();
consumer.join().unwrap();
```

### When *not* to reach for crossbeam

- **Async I/O.** Use [[tokio]] instead. Crossbeam's primitives are sync — calling `.recv()` blocks an OS thread.
- **Embarrassingly parallel data work.** Use [`rayon`](https://crates.io/crates/rayon)'s `par_iter()`; it's built on crossbeam-deque under the hood and gives you a much higher-level API.
- **Single-producer single-consumer at very high rates.** [`spsc_queue`](https://docs.rs/spsc-queue/) or a custom ring buffer can be measurably faster.

### Useful entry points

- Repo / workspace: <https://github.com/crossbeam-rs/crossbeam>
- Channel docs: <https://docs.rs/crossbeam-channel>
- "Crossbeam channels" benchmark notes: <https://github.com/crossbeam-rs/crossbeam/blob/master/crossbeam-channel/benchmarks/README.md>
- `std::thread::scope` (the stdlib equivalent of `crossbeam::scope`): <https://doc.rust-lang.org/std/thread/fn.scope.html>
