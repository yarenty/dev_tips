---
title: "tickgrinder: Building an Algorithmic Trading Platform in Rust"
main_link: https://cprimozic.net/blog/building-an-algorithmic-trading-platform-in-rust/
keywords: [tickgrinder, rust, trading, futures-rs, async, redis-pubsub, postgresql]
status: reviewed
review_date: 2026/05/03
---

# tickgrinder: Building an Algorithmic Trading Platform in Rust

**Main link:** <https://cprimozic.net/blog/building-an-algorithmic-trading-platform-in-rust/>

Repo (archived): <https://github.com/ameobea/tickgrinder>

## Summary

A 2017-era write-up by [Casey Primozic / @ameobea](https://cprimozic.net/) on his fourth attempt at building a Rust algorithmic trading platform from scratch. The architecture is microservice-flavoured but in-process: a **tick processor** evaluates per-tick conditions and acts on them; an **optimizer** is the brain that decides what conditions the tick processors should evaluate (raw prices, indicators, ML); a **spawner / instance manager** brings instances up and down; and an **MM** (management/monitoring) module is the human-facing entry point. Modules talk to each other over a custom protocol on top of Redis pub/sub, and persist to PostgreSQL. The whole thing leans hard on the (now-historical) `futures-rs` crate for async composition, with worker threads handling blocking calls behind futures channels.

## Insight

This article is more interesting as a *historical artifact* than as a recipe to follow today. It predates `async`/`await` by years, so the "asynchronous programming" section is full of patterns — `Sender::send` consuming itself and yielding a future for the new sender, `Oneshot`s as a primitive timeout signal, manual worker threads behind every blocking call — that are now subsumed by `tokio` and modern `async fn`. If you want to understand *why* async Rust looks the way it does, this is a window into the world it grew out of.

The architectural choices are worth contrasting with [[folbrecht_algo_trading_series]] (sync, single process, channels, no Redis) and [[barter]] (modern async, in-process traits, no external bus). Tickgrinder reaches for Redis pub/sub + PostgreSQL essentially because the author wanted true module isolation; both Folbrecht and Barter argue you don't need that until you do, and crossbeam channels or mpsc are enough.

## Similar / related topics

- [[folbrecht_algo_trading_series]] — sync, hand-rolled, no external broker; intentional opposite.
- [[barter]] — modern, async, in-process trait wiring; the "what tickgrinder would look like today".
- [[hummingbot_python]] — Python equivalent with a similar plug-in module shape.
- [[db/nosql/redis|Redis]] — pub/sub bus used here for inter-module messaging.
- [[db/relational/postgresql|PostgreSQL]] — main storage backend.

## Internal links

- [[folbrecht_algo_trading_series]] — sync, channels-only counterpart
- [[barter]] — modern async Rust trading framework
- [[db/nosql/redis|Redis]] — pub/sub used as the message bus
- [[db/relational/postgresql|PostgreSQL]] — primary persistence
- [[tokio]] — modern replacement for `futures-rs` patterns shown here

## Keywords

`#trading` `#rust` `#tickgrinder` `#futures-rs` `#async` `#redis-pubsub` `#postgresql` `#microservices`

## References / raw notes

Companion blog post (also linked by the author): <https://medium.com/@fintechdevlondon/i-built-an-automated-trading-system-in-rust-for-a-hedge-fund-in-6-months-dfe027fc14ec>

### Architecture (paraphrased from the post)

- **Tick processor(s)** — consume tick streams (timestamp + symbol + bid/ask) from the broker; evaluate the optimizer-supplied conditions; on match, open / close / modify positions.
- **Optimizer** — the "brain". Pulls in prices, indicators, ML outputs, decides what conditions the tick processors should fire on. Designed to be heavy-CPU and to live off the hot path of the tick processors.
- **Spawner / Instance Manager** — entry point of the platform; spawns and reaps module instances; the first thing it does after init is start an MM.
- **MM (Management / Monitoring)** — human UI: start trading, kick off backtests, view status, render charts of internal indicators and live prices.
- **Bus**: custom protocol on top of **Redis pub/sub**. **Storage**: **PostgreSQL**.

### The "futures-rs" pattern, in 2017 idioms

The post leans on a pattern that's worth remembering as an artifact: blocking I/O lives in worker threads, results are pushed onto a futures channel, and the rest of the application chains futures off that channel. The author's `sub_channel` example:

```rust
/// Returns a Receiver that resolves to new messages received on a pubsub channel
pub fn sub_channel(host: &str, ps_channel: &'static str) -> Receiver<String, ()> {
    let (tx, rx) = channel::<String, ()>();
    let ps = get_pubsub(host, ps_channel);
    let mut new_tx = tx;
    thread::spawn(move || {
        while let Ok(_new_tx) = get_message_outer(new_tx, &ps) {
            new_tx = _new_tx;
        }
        println!("Channel subscription expired");
    });
    rx
}
```

Two things in there that no longer exist in modern Rust:

1. `Stream::send` *consumed* the sender and returned a future for a new sender — so `get_message_outer` had to thread the new `Sender` back out on every iteration.
2. Timeouts were implemented as a long-lived "timeout server" thread you sent `(Duration, Oneshot)` requests to. Today: `tokio::time::timeout` or `futures::FutureExt::timeout`.

The big-picture takeaway is the same one Folbrecht arrives at from the opposite direction: parallelism in trading systems is *task-shaped*, not connection-shaped, so there's no need for tens-of-thousands of concurrent tasks. Whether you express that with sync threads + channels (Folbrecht) or with futures + worker threads (tickgrinder) is largely taste.
