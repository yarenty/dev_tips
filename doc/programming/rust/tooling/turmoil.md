---
title: turmoil — Tokio's deterministic distributed-system simulator
main_link: https://github.com/tokio-rs/turmoil
keywords: [turmoil, rust, tokio, distributed-systems, simulation, testing, deterministic]
status: reviewed
review_date: 2026/05/03
---

# turmoil — Tokio's deterministic distributed-system simulator

**Main link:** <https://github.com/tokio-rs/turmoil>

## Summary

`turmoil` is a Rust framework by the Tokio team for testing distributed systems with **deterministic execution**. It runs multiple "hosts" as concurrent tasks within a single thread, intercepts their network I/O, and lets your test introduce hardship — partitions, delays, drops, packet reordering — either deterministically (seeded RNG) or with explicit `partition`/`hold`/`release` calls. The whole simulation is single-threaded so an entire failure scenario is reproducible from a seed.

Announcement: <https://tokio.rs/blog/2023-01-03-announcing-turmoil>.

## Insight

Reach for `turmoil` when you're building anything that talks to peers over the network and you want to test "what happens if node B sees C's messages before A's?" without standing up a real cluster. Standard pattern:

```rust
use turmoil::Builder;

let mut sim = Builder::new().build();

sim.client("client", async {
    // makes "outgoing" connections via turmoil's tokio shim
    let mut sock = turmoil::net::TcpStream::connect("server:80").await?;
    // ...
    Ok(())
});

sim.host("server", || async {
    let listener = turmoil::net::TcpListener::bind("0.0.0.0:80").await?;
    // ...
    Ok(())
});

// Now interrogate / disrupt the network:
sim.partition("client", "server");          // drop all packets between the two
sim.run()?;                                  // step the sim until clients return
```

**Critical caveat**: turmoil only sees network I/O that goes through its own `turmoil::net::*` shims (TCP, UDP). You have to write your code against those types or behind a trait that you swap in tests. Library code that hardcodes `tokio::net::TcpStream` cannot be turmoil-tested without modification — this is the equivalent of "all I/O must be injected" you'd do anyway for unit testability.

**Compared to siblings**:

- **[`madsim`](https://github.com/madsim-rs/madsim)** — broader scope: simulates the whole tokio runtime + filesystem + RNG; used by RisingWave team. More invasive setup but more thorough.
- **[`shuttle`](https://github.com/awslabs/shuttle)** — AWS-Labs randomised tester for concurrent code; covers data races, not network partitions.
- **[`loom`](https://github.com/tokio-rs/loom)** — exhaustively explores interleavings of *atomics and locks* in a small concurrent test (different niche; unsafe-correctness, not network).
- **`jepsen`** (Clojure) — the famous test framework that tortures real distributed systems from outside; orthogonal — turmoil tests *your code's logic* in-process, jepsen tests *the deployed system*.

## Similar / related topics

- [`madsim`](https://github.com/madsim-rs/madsim) — broader simulator (whole runtime).
- [`shuttle`](https://github.com/awslabs/shuttle) — concurrency randomiser (AWS Labs).
- [`loom`](https://github.com/tokio-rs/loom) — atomics/locks model checker.
- `jepsen` — black-box external distributed-system tester.

## Internal links

- [[README]] — tooling section landing.
- [[tests]] — broader Rust testing landscape.
- [[../concurrency/tokio|concurrency/tokio]] — the runtime turmoil simulates.

## Keywords

`#turmoil` `#rust` `#tokio` `#distributed-systems` `#simulation` `#testing` `#deterministic`

## References / raw notes

- Repo: <https://github.com/tokio-rs/turmoil>
- Crate: <https://crates.io/crates/turmoil>
- Announcement: <https://tokio.rs/blog/2023-01-03-announcing-turmoil>
- Single-threaded sim of multiple hosts; controllable network; seedable RNG for reproducibility.
