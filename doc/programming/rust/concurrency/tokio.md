---
title: "tokio: the canonical async runtime for Rust"
main_link: https://tokio.rs/
keywords: [tokio, rust, async, runtime, concurrency, mio, futures]
status: reviewed
---

# tokio: the canonical async runtime for Rust

**Main link:** <https://tokio.rs/>

Repo: <https://github.com/tokio-rs/tokio>

## Summary

[Tokio](https://tokio.rs/) is the dominant async runtime for Rust. It provides the executor that drives `async fn` to completion, an OS-event-loop ([`mio`](https://github.com/tokio-rs/mio)) for non-blocking I/O, and async equivalents of stdlib primitives — `tokio::net::TcpStream`, `tokio::time::sleep`, `tokio::sync::{mpsc, oneshot, broadcast, watch, Mutex, RwLock, Notify}`, `tokio::fs`, `tokio::process`, `tokio::signal`. The whole crates.io async ecosystem (hyper, axum, reqwest, sqlx, tonic, ...) is built on top.

## Insight

Tokio is the right answer when you have many concurrent I/O-bound tasks (10k web requests, 1k websocket subscribers, a TCP proxy, a database connection pool). It is the *wrong* answer for CPU-bound work — those go on a dedicated thread pool ([`rayon`](https://crates.io/crates/rayon) or `tokio::task::spawn_blocking`) so they don't starve the runtime's worker threads.

Three things to internalise so you stop fighting it:

1. **`async fn` does nothing until awaited.** A future is a value, not a running task. To run it concurrently, hand it to `tokio::spawn(...)` or `join!(...)` / `select!(...)`. This is the source of most "why isn't my code running?" confusion.
2. **Don't hold a `std::sync::Mutex` across an `.await`.** It blocks the runtime worker, and if the worker is blocked the whole runtime is. Use `tokio::sync::Mutex`, or restructure so the lock is released before the await point.
3. **Cancellation is everywhere.** Drop a future and it's cancelled mid-poll. This is genuinely useful (timeouts via `tokio::time::timeout`, racing via `select!`) but it means async fns must be cancellation-safe — partial state on cancellation is your problem.

The runtime ships in two flavours: **multi-threaded** (default for `#[tokio::main]`, work-stealing across N OS threads) and **current-thread** (single-threaded, useful for tests, embedded, or when you genuinely want one task at a time). Pick multi-threaded unless you have a reason.

## Similar / related topics

- [`async-std`](https://async.rs/) — alternative runtime, mostly stdlib-shaped API; less popular than Tokio in 2024 and many crates only target Tokio.
- [`smol`](https://github.com/smol-rs/smol) — small, modular runtime; good for embedded or when you want to assemble pieces.
- [`mio`](https://github.com/tokio-rs/mio) — the low-level OS event loop Tokio is built on; useful if you're writing your own runtime or need to integrate with epoll/kqueue/IOCP directly.
- [[crossbeam]] — channels for the *sync* world; the natural choice when you'd rather not adopt async.
- [[barter]] — example of a real codebase built on Tokio (event-driven trading framework).

## Internal links

- [[crossbeam]] — sync-world counterpart for channels and threading primitives
- [[barter]] — production codebase built on Tokio
- [[folbrecht_algo_trading_series]] — explicitly avoids Tokio; useful contrast on when async is and isn't justified

## Keywords

`#rust` `#concurrency` `#tokio` `#async` `#runtime` `#mio` `#futures`

## References / raw notes

### "Hello, world" with Tokio

```rust
#[tokio::main]
async fn main() {
    let body = reqwest::get("https://api.github.com/zen")
        .await
        .unwrap()
        .text()
        .await
        .unwrap();
    println!("{body}");
}
```

`#[tokio::main]` expands to a `fn main()` that constructs a multi-threaded runtime and blocks on the body. Equivalent without the macro:

```rust
fn main() {
    let rt = tokio::runtime::Builder::new_multi_thread()
        .enable_all()
        .build()
        .unwrap();
    rt.block_on(async {
        // ... your async code ...
    });
}
```

### Spawning, join, select

```rust
use tokio::time::{sleep, Duration};

let h1 = tokio::spawn(async { sleep(Duration::from_millis(50)).await; 1 });
let h2 = tokio::spawn(async { sleep(Duration::from_millis(80)).await; 2 });

let (a, b) = tokio::try_join!(h1, h2).unwrap();   // concurrent, both must succeed
println!("{a:?} {b:?}");                          // (1, 2)

// Race: take whichever finishes first; the loser is cancelled
let winner = tokio::select! {
    _ = sleep(Duration::from_millis(50)) => "fast",
    _ = sleep(Duration::from_millis(80)) => "slow",
};
```

### CPU-bound work belongs on `spawn_blocking`

```rust
let result = tokio::task::spawn_blocking(|| expensive_sync_thing())
    .await
    .unwrap();
```

Or push it to [`rayon`](https://crates.io/crates/rayon) and bridge the result back via a `tokio::sync::oneshot`.

### Useful entry points

- Docs: <https://docs.rs/tokio>
- Tutorial ("Mini-Redis"): <https://tokio.rs/tokio/tutorial>
- Async book: <https://rust-lang.github.io/async-book/>
- Why "async cancellation" is subtle: <https://blog.yoshuawuyts.com/cancellation-async-programming/>
