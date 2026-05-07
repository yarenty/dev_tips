---
title: tinyvec — 100% safe small-vector types
main_link: https://crates.io/crates/tinyvec
keywords: [tinyvec, smallvec, arrayvec, vec, no-std, safe, lokathor, rust]
status: reviewed
---

# tinyvec — 100% safe small-vector types

**Main link:** <https://crates.io/crates/tinyvec>

## Summary

`tinyvec` is a `#![forbid(unsafe_code)]` crate of `Vec`-like data structures by [@Lokathor](https://github.com/Lokathor). The headline types are:

- **`ArrayVec<[T; N]>`** — array-backed `Vec`-like; pushes past capacity panic.
- **`SliceVec<&mut [T]>`** — same, but borrowing a `&mut [T]` instead of owning it.
- **`TinyVec<[T; N]>`** (with the `alloc` feature) — an enum that's either `Inline(ArrayVec)` or `Heap(Vec)`; transparently transitions to the heap on overflow.

The price for the "100% safe" claim: **`T` must implement `Default`**. Tinyvec uses default values to fill the unused slots of the inline array, sidestepping the `MaybeUninit` / `unsafe` gymnastics that `smallvec` does instead.

## Insight

The choice between `tinyvec` and `smallvec` is the standing question:

| Crate                                                    | Trade-off                                           |
|----------------------------------------------------------|-----------------------------------------------------|
| **`tinyvec`**                                            | 100% safe, requires `T: Default`, no `unsafe` audit cost. |
| [**`smallvec`**](https://crates.io/crates/smallvec)      | No `T: Default` requirement, uses `unsafe` internally; soundness bugs have happened (and been fixed) over the years. |
| `Vec<T>`                                                 | Default. Don't optimise unless you know the small case dominates. |
| `[T; N]` + `len: usize`                                  | Hand-rolled; what tinyvec generates for you.        |
| `heapless::Vec<T, N>` (no_std embedded world)            | When you can never allocate.                        |

Reach for one of these when **profiling shows lots of small `Vec`s being heap-allocated** — call-stack-allocated parser results, AST node children, query plan nodes, per-pixel candidate lists, etc. The win is twofold: skipping the allocator (a big deal in tight loops) and getting better cache locality (the small payload sits next to its length on the stack). For the embedded / `no_std` world `tinyvec` is a natural fit because the safe-only constraint matches the audit posture you usually want there.

Gotchas:

- **`T: Default` rules out** `Box<dyn Trait>`, FFI handles, anything `!Default` from a third-party crate. In those cases you're stuck with `smallvec` or `Vec`.
- **`ArrayVec` panics on overflow**, it doesn't allocate. Use `TinyVec` (the enum) if you want auto-promotion.
- **Inline capacity isn't free** — `tinyvec![T; 256]` puts a 256-element array on every stack frame that holds one. Tune the inline capacity to your actual P50/P95.
- **Iteration over `TinyVec`** dispatches through the enum every time; for hot loops, match once and iterate the inner backend.

## Similar / related topics

- [smallvec](https://crates.io/crates/smallvec) — Servo's small-vector crate; uses `unsafe`, works for any `T`.
- [arrayvec](https://crates.io/crates/arrayvec) — the original safe `ArrayVec`; tinyvec borrows the type name.
- [heapless](https://crates.io/crates/heapless) — `no_std` collections that never allocate; embedded standard.
- [[halfbrown]] — the same "small inline, switch to heavy when bigger" pattern, but for `HashMap`.
- [[hashbrown]] — the "heavy" backend halfbrown switches to, for context.

## Internal links
<!-- reviewed -->
- [[README]]
- [[halfbrown]]
- [[hashbrown]]

## Keywords

`#rust` `#tinyvec` `#smallvec` `#arrayvec` `#data-structures` `#no-std` `#performance`

## References / raw notes

- Crate: <https://crates.io/crates/tinyvec>
- Repo: <https://github.com/Lokathor/tinyvec>

A 100% safe crate of vec-like types — `#![forbid(unsafe_code)]`. Main types:

- **`ArrayVec`** — array-backed vec-like data structure. Panics on overflow.
- **`SliceVec`** — same idea, but using a `&mut [T]`.
- **`TinyVec`** (with the `alloc` feature) — an enum that's either `Inline(ArrayVec)` or `Heap(Vec)`. If a `TinyVec` is `Inline` and would overflow, it automatically transitions to `Heap` and continues whatever it was doing.

To attain the "100% safe code" status there is one compromise: the element type of the vecs must implement `Default`.
