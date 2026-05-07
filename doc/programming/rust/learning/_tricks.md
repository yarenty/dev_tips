---
title: println! 5x speedup trick (capture-by-value formatter)
main_link: https://users.rust-lang.org/
keywords: [tricks, rust, println, format, performance, display, tczajka]
status: reviewed
review_date: 2026/05/03
---

# println! 5x speedup trick (capture-by-value formatter)

**Main link:** <https://users.rust-lang.org/>

## Summary

A micro-optimisation curiosity from the users.rust-lang.org forum (attributed to user `tczajka`): inside a tight loop, formatting `println!("{} {}", x, y)` can be ~5× slower than `println!("{} {}", {x}, {y})` because the block expression `{x}` *moves/copies* the value into the macro's argument slot, freeing the compiler from worrying that a `Display` impl with shared-reference access could observe a change to `x` mid-call. With the explicit copy, the compiler can hoist invariant work out of the loop.

## Insight

This is a real but **highly situational** speedup — it only matters in the rare case that you're calling `println!` (or any `format!`-family macro) inside a tight hot loop and the formatted values are `Copy`. For everyday code the difference is invisible. The deeper lesson is what it reveals about Rust's aliasing model: `Display::fmt(&self, …)` takes `&self`, and the compiler can't statically prove that no upstream code holds a `&mut` to the same value, so it conservatively re-fetches per call. Forcing a by-value capture makes that question moot. The captured-identifier syntax `println!("{x} {y}")` (stabilised in Rust 1.58) is *also* by-value semantically and gets the same treatment, with cleaner syntax.

## Similar / related topics

- [`std::hint::black_box`](https://doc.rust-lang.org/std/hint/fn.black_box.html) — for benchmarking the actual difference yourself.
- [`criterion`](https://github.com/bheisler/criterion.rs) — the standard benchmarking crate to verify micro-claims.
- [The Rust Performance Book](https://nnethercote.github.io/perf-book/) — broader playbook for these kinds of tweaks.
- Rust 1.58 release notes — captured-identifier `println!("{x}")` syntax.
- [`std::fmt`](https://doc.rust-lang.org/std/fmt/) — the `Display` / `Debug` machinery being optimised here.

## Internal links
- [[cargo_toml]]
- [[cheats]]
- [[README]]

## Keywords

`#tricks` `#rust` `#println` `#format` `#performance` `#display`

## References / raw notes

Attributed to forum user `tczajka`. The trick:

```rust
// Slower in a tight loop:
println!("{} {} {} {} {}", x1, x2, x3, x4, x5);

// ~5x faster:
println!("{} {} {} {} {}", {x1}, {x2}, {x3}, {x4}, {x5});

// Equivalent modern syntax (Rust 1.58+), same effect:
println!("{x1} {x2} {x3} {x4} {x5}");
```

The original explanation: the compiler is worried that the variables may get changed through immutable references passed to `Display` implementations, making the `f` calls not invariant across loop iterations. The `{x}` block forces a value copy, removing that aliasing concern.
