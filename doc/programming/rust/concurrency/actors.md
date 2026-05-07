---
title: "Actix and the Rust actor-model landscape"
main_link: https://crates.io/crates/actix
keywords: [actors, rust, actix, ractor, message-passing, concurrency, tokio]
status: reviewed
---

# Actix and the Rust actor-model landscape

**Main link:** <https://crates.io/crates/actix>

Repo: <https://github.com/actix/actix>

## Summary

[`actix`](https://crates.io/crates/actix) is the original Rust actor framework — typed messages, supervised actors, an `Addr<A>` handle you `send` / `do_send` to, all running on top of [[tokio]]. The same project also produces the much-better-known [`actix-web`](https://actix.rs/), but the web framework long ago stopped requiring the actor crate; today the actor library is a smaller, somewhat-quieter dependency that mostly powers existing apps. Newer alternatives — **ractor**, **kameo**, **coerce**, **xtra**, **riker** — explore the same shape with more idiomatic async APIs.

## Insight

The honest framing: in modern Rust the actor model is "[[tokio]] + a stricter ownership pattern". A `tokio::spawn` task that owns some state and reads commands off an `mpsc::Receiver<Message>` *is* an actor — `send` / `do_send` / `Addr` are syntactic sugar around exactly that. So most projects that *would* have reached for an actor framework five years ago now just write that pattern by hand. Reach for an actor crate when you specifically want:

- **Typed message dispatch** — one `Handler<M>` impl per message type instead of a giant `match` in your task loop.
- **Supervision trees** — restart-on-panic semantics borrowed from Erlang; useful if you have many short-lived workers and want crash-tolerance without writing the bookkeeping.
- **Ask-style request/response** — `addr.send(Msg)` returns a future for the reply, vs. having to wire an `oneshot` by hand.

Gotchas worth knowing:

1. **`actix` ≠ `actix-web`.** `actix-web` is the popular web framework; the actor crate is a separate, smaller dependency. Don't pull in `actix` thinking you need it for HTTP.
2. **Bring-your-own-runtime is awkward.** `actix` runs on its own `System` (a thin wrapper over a current-thread Tokio runtime). Mixing it with a standard `#[tokio::main]` multi-thread runtime takes care.
3. **The "ractor" school is more idiomatic Rust.** [`ractor`](https://github.com/slawlor/ractor) (by LinkedIn) and [`kameo`](https://github.com/tqwewe/kameo) lean into `async fn handle(...)`, drop the macro-heavy registration, and integrate cleanly with regular Tokio code. If you're starting today and you want actors, look there first.
4. **You probably don't need actors.** Channels + `tokio::spawn` + a `select!` loop covers ~90% of the use cases; reach for a framework only when you find yourself rebuilding supervision or typed-dispatch by hand.

## Similar / related topics

- [`ractor`](https://github.com/slawlor/ractor) — modern Tokio-native actor framework from LinkedIn; Erlang-style supervision, async handlers, no system-runtime ceremony.
- [`kameo`](https://github.com/tqwewe/kameo) — newer minimal actor library; focuses on ergonomic `async fn` handlers and remote actors via `libp2p`.
- [`coerce`](https://github.com/leonhartley/coerce-rs) — distributed actors with built-in clustering and persistence.
- [`xtra`](https://github.com/Restioson/xtra) — small "tiny actor" crate; a thin layer over `tokio::spawn` + channels.
- [Riker](https://riker.rs/) — older Akka-shaped framework; less active.
- [[tokio]] — what every Rust actor framework is built on; channels + `spawn` is the pattern actor crates are sugar over.

## Internal links

<!-- reviewed -->

- [[tokio]] — the runtime under every actor crate; "actors" is a pattern *on top of* this.
- [[crossbeam]] — sync-world counterpart; for thread-based message passing without async.
- [[concurrency/README|Rust concurrency]] — landing page with the workload-→-tool decision table.

## Keywords

`#rust` `#concurrency` `#actors` `#actix` `#ractor` `#message-passing` `#tokio`

## References / raw notes

### Actix features (from upstream README)

- Async and sync actors
- Local / thread-context actor communication
- Futures-based async message handling
- Actor supervision
- Typed messages (no `Any`)
- Stable Rust

### Add to `Cargo.toml`

```toml
[dependencies]
actix = "0.13"
```

### Define a `System` (required entry point)

```rust
fn main() {
    let system = actix::System::new();
    system.run().unwrap();
}
```

`System::new()` creates a current-thread Tokio runtime; `system.run()` blocks until the system is told to stop (typically via `System::current().stop()`).

### Implement an actor

```rust
use actix::{Actor, Context, System};

struct MyActor;

impl Actor for MyActor {
    type Context = Context<Self>;

    fn started(&mut self, _ctx: &mut Self::Context) {
        println!("I am alive!");
        System::current().stop();
    }
}

fn main() {
    let mut system = System::new();
    let _addr = system.block_on(async { MyActor.start() });
    system.run().unwrap();
}
```

`started` / `stopping` / `stopped` are the lifecycle hooks; see the [actix book](https://actix.rs/docs/actix/) for the full state machine.

### Typed messages with a reply

```rust
use actix::prelude::*;

#[derive(Message)]
#[rtype(result = "usize")]
struct Sum(usize, usize);

struct Calculator;

impl Actor for Calculator {
    type Context = Context<Self>;
}

impl Handler<Sum> for Calculator {
    type Result = usize;

    fn handle(&mut self, msg: Sum, _ctx: &mut Context<Self>) -> Self::Result {
        msg.0 + msg.1
    }
}

#[actix::main]
async fn main() {
    let addr = Calculator.start();
    let res = addr.send(Sum(10, 5)).await;
    match res {
        Ok(result) => println!("SUM: {result}"),
        _ => println!("Communication to the actor has failed"),
    }
}
```

`send` returns a `Future` for the reply; `do_send` is fire-and-forget. `Recipient<M>` lets you erase the actor type when you only care about the message type.

### Useful entry points

- Actix book: <https://actix.rs/docs/actix/>
- Crate: <https://crates.io/crates/actix>
- Repo: <https://github.com/actix/actix>
- Modern alternative — ractor: <https://github.com/slawlor/ractor>
- Modern alternative — kameo: <https://github.com/tqwewe/kameo>
