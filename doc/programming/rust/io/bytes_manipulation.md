---
title: "bytemuck — safe bit-cast between POD types"
main_link: https://crates.io/crates/bytemuck
keywords: [rust, bytemuck, bytes, zero-copy, pod, transmute, byteorder]
status: reviewed
review_date: 2026/05/03
---

# bytemuck — safe bit-cast between POD types

**Main link:** <https://crates.io/crates/bytemuck>

## Summary

[`bytemuck`](https://crates.io/crates/bytemuck) is a tiny crate by Lokathor that lets you safely **reinterpret the bits** of one value as another value of a different type — the safe-Rust equivalent of `std::mem::transmute`, gated by marker traits (`Pod`, `Zeroable`, `NoUninit`, `AnyBitPattern`) so the compiler can refuse the conversion when it isn't sound. It's the foundation under crates that move byte buffers in and out of typed structures (graphics vertex buffers, networking frames, on-disk records) without `unsafe` at the call site.

## Insight

Reach for bytemuck when you have a `&[u8]` and want a `&[MyStruct]` (or vice-versa) and your struct is **plain-old-data** — fixed layout, no padding holes, no pointers, no `Drop`. The classic use case is GPU / wgpu vertex buffers, but it also covers binary protocol parsing, mmap'd files, and any time you'd otherwise reach for `transmute`.

How it slots vs. neighbours:

- **vs. `std::mem::transmute`** — same operation, but `transmute` is `unsafe` at every call site, whereas bytemuck moves the safety obligation up to the trait `impl` (typically derived). You write `#[derive(Pod, Zeroable)]` once and never touch `unsafe` again.
- **vs. [`zerocopy`](https://crates.io/crates/zerocopy)** — Google's competitor with similar goals. zerocopy has stronger checks for padding/alignment and more conservative defaults; bytemuck is older, smaller, more widely depended-on (wgpu, glam, etc.). Pick zerocopy for new-design networking code where you want belt-and-braces; pick bytemuck when you're matching the wider Rust graphics ecosystem.
- **vs. [`byteorder`](https://crates.io/crates/byteorder)** — different problem. `byteorder` reads/writes integers with explicit endianness from a `Read`/`Write` stream. bytemuck is about whole-struct reinterpretation. They compose: parse the framing with byteorder, then `cast_slice` the payload with bytemuck.
- **vs. the [`bytes`](https://crates.io/crates/bytes) crate** — also unrelated. `bytes` is Tokio-adjacent reference-counted buffer storage (`Bytes`, `BytesMut`); bytemuck is the *contents-typing* layer that can sit on top of any byte storage including `Bytes`.

Gotchas:

1. **Padding bytes are forbidden in `Pod`.** A struct like `(u8, u32)` has 3 bytes of padding and won't derive `Pod` — you need explicit `#[repr(C)]` and manual padding fields, or use `NoUninit` + `AnyBitPattern` separately.
2. **Endianness is not handled.** bytemuck reinterprets bytes as-is; if your on-disk format is big-endian and your CPU is little-endian, you still need byteorder (or `u32::from_be`).
3. **Alignment matters.** `cast_slice::<u8, u32>(&[u8])` will panic at runtime if the underlying slice isn't 4-byte aligned. Use `try_cast_slice` if the input alignment is not under your control.

## Similar / related topics

- [`zerocopy`](https://crates.io/crates/zerocopy) — Google's safer-bit-cast crate with stricter padding/alignment audits.
- [`byteorder`](https://crates.io/crates/byteorder) — endianness-aware integer read/write over `Read` / `Write`.
- [`bytes`](https://crates.io/crates/bytes) — Tokio's reference-counted byte buffer (`Bytes` / `BytesMut`); orthogonal to bytemuck and often paired with it.
- [`bitfrob`](https://crates.io/crates/bitfrob) — sister crate by the same author for sub-byte bit-twiddling (extracting bitfields, etc.).
- [`safe-transmute`](https://crates.io/crates/safe-transmute) — older, more general-purpose safe-transmute crate; less ergonomic than bytemuck today.

## Internal links

- [[programming/rust/io/README|Rust I/O]] — section landing page.
- [[programming/rust/io/parsers|parsers]] — when you need *structured* parsing instead of raw bit-cast.
- [[programming/rust/io/json|json]] — schema-driven (de)serialization counterpart to bit-cast.

## Keywords

`#rust` `#bytemuck` `#bytes` `#zero-copy` `#pod` `#transmute` `#byteorder`

## References / raw notes

### bytemuck

A crate for mucking around with piles of bytes.

This crate lets you safely perform "bit cast" operations between data types. That's where you take a value and just reinterpret the bits as being some other type of value, without changing the bits.

- Not like the `as` keyword (which converts numerically).
- Not like the `From` trait (which converts semantically).
- Most like `f32::to_bits`, just generalized to convert between arbitrary POD types.

<https://crates.io/crates/bytemuck>

### bitfrob

Sister crate for sub-byte bit-fiddling utilities.

<https://crates.io/crates/bitfrob>
