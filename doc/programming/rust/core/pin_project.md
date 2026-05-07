---
title: pin-project — safe pin-projection
main_link: https://crates.io/crates/pin-project
keywords: [pin-project, pin, async, futures, taiki-e, rust, self-referential]
status: reviewed
---

# pin-project — safe pin-projection

**Main link:** <https://crates.io/crates/pin-project>

## Summary

`pin-project` is a procedural-macro crate by [@taiki-e](https://github.com/taiki-e) that derives **safe, ergonomic pin-projection** for structs and enums — the operation of going from `Pin<&mut MyStruct>` to `Pin<&mut Field>` (or `&mut Field`) without writing the unsafe boilerplate by hand. It's the standard tool any time you implement `Future` or `Stream` by hand and need to project to a child future, and it's a near-universal dependency in the async ecosystem.

```toml
pin-project = "1"
```

## Insight

You only ever need this when **implementing a `Future` (or `Stream`/`AsyncRead`/etc.) by hand on a struct with poll-able children**. Day-to-day async code with `async fn` never sees `Pin` at all. When you do hit it, the choice is between three options:

| Option                 | When                                                        |
|------------------------|-------------------------------------------------------------|
| **`pin-project`**      | Default. Fully featured (`#[pin]` per field, `#[pinned_drop]`, projection on enums and `&Pin<&mut Self>` and `Pin<&Self>`). Adds a proc-macro dep. |
| **`pin-project-lite`** | Same author. **No proc-macro** (it's a `macro_rules!` macro), so much faster compile times and smaller dep tree. Slightly more restrictive syntax. Used inside `tokio` itself. |
| Hand-written `unsafe`  | One-off, you understand the [`Pin` invariants](https://doc.rust-lang.org/std/pin/index.html#projections-and-structural-pinning), don't want a dep. |

The mental model: **pinning is structural** — if you decide a field is pinned (`#[pin]`), every projection respects that, and you must not move out of the field; if you don't pin it, you can `&mut` project freely. The macro generates a `Projection<'_>` struct whose fields are `Pin<&mut T>` for pinned fields and `&mut T` for unpinned ones. The whole point is that you write `let this = self.project(); this.future.poll(cx)` and the compiler enforces the invariants for you.

Gotchas:

- **`#[pinned_drop]` is required if any field is `#[pin]` and the type implements `Drop`** — the standard `Drop` would give you `&mut self`, breaking pinning. The macro emits a compile error reminding you.
- **Compile-time cost** — the proc-macro version isn't free; if you have many such structs, prefer `pin-project-lite`.
- **`!Unpin` propagation** — pinning a field does not magically make the parent `!Unpin`; you still need `PhantomPinned` (which the macro can add automatically with `#[pin_project(!Unpin)]`).
- **No need in `async fn`** — the compiler-generated state machine handles all of this for you. You only need pin-project when *manually* writing the `poll()` body.

## Similar / related topics

- [pin-project-lite](https://crates.io/crates/pin-project-lite) — declarative-macro version, no proc-macro dep, used by tokio.
- [`std::pin`](https://doc.rust-lang.org/std/pin/) — the standard-library primitives the crate builds on.
- [futures](https://crates.io/crates/futures) — the place you most often need to project (`Stream`, `Sink`, combinators).
- *Async Rust* by Maxwell Flitton & Caroline Morton — has the clearest narrative explanation of pinning I've read.

## Internal links
<!-- reviewed -->
- [[README]]
- [[../concurrency/README|concurrency]]

## Keywords

`#rust` `#pin-project` `#pin` `#async` `#futures` `#unsafe`

## References / raw notes

- Crate: <https://crates.io/crates/pin-project>
- Docs: <https://docs.rs/pin-project/>
- Lite version: <https://crates.io/crates/pin-project-lite>
- `std::pin` reference: <https://doc.rust-lang.org/std/pin/>

A crate for safe and ergonomic pin-projection. Standard pattern in hand-rolled `Future` impls:

```rust
use pin_project::pin_project;
use std::pin::Pin;
use std::task::{Context, Poll};
use std::future::Future;

#[pin_project]
struct Wrap<F> {
    #[pin]
    inner: F,
    counter: u32,
}

impl<F: Future> Future for Wrap<F> {
    type Output = F::Output;

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output> {
        let this = self.project();
        *this.counter += 1;
        this.inner.poll(cx)
    }
}
```
