---
title: External UDFs (DataFusion + Arrow FFI)
main_link: https://lib.rs/crates/dlopen2
keywords: [external-udfs, udf, ffi, arrow, datafusion, dlopen, c-abi, rust, zero-copy]
status: reviewed
---

# External UDFs (DataFusion + Arrow FFI)

**Main link:** <https://lib.rs/crates/dlopen2>

`dlopen2`: <https://lib.rs/crates/dlopen2> ·
`dlopen2_derive`: <https://lib.rs/crates/dlopen2_derive> ·
Arrow FFI: <https://docs.rs/arrow/latest/arrow/ffi/index.html>

## Summary

This article sketches a design for **external UDFs** — user-defined functions
that live in a *separately compiled shared library* (`.so` / `.dylib` /
`.dll`) and are loaded into a query engine like Apache DataFusion at runtime.
The boundary uses the **standard C ABI** (so the library can be written in
any language) and the **Arrow C Data Interface** for zero-copy passing of
columnar batches. The big wins versus naive in-process UDFs are: no Rust ABI
instability problem, no serialisation overhead, polyglot implementations.
The big cost is `unsafe`: the boundary is FFI, and you own correctness
yourself.

## Insight

When to reach for this pattern:

- **You have a Rust-based engine** (DataFusion is the obvious case) and want
  users to drop in compiled UDFs *without* recompiling the engine.
- **You want polyglot UDFs** — the boundary is C ABI, so a `.so` written in
  C, C++, Zig, or Rust all work the same.
- **You want zero-copy**: Arrow's C Data Interface lets you hand columnar
  batches across the FFI boundary as `(FFI_ArrowArray, FFI_ArrowSchema)`
  pointer pairs without serialising.

Why this design over the alternatives:

- **vs in-process Rust trait UDFs** — those require the UDF crate to be
  compiled with the same `rustc` and the same DataFusion version as the
  engine. Rust has no stable ABI, so this is brittle. C ABI sidesteps it
  entirely.
- **vs WASM UDFs (SingleStore-style)** — WASM is sandboxed and portable, but
  has higher per-call overhead and needs a runtime. C ABI is faster but
  trusts the library not to crash.
- **vs RPC / sidecar UDFs** — RPC is network-shaped, serialises everything,
  and dies for any high-fanout call pattern. FFI is in-process and zero-copy.

Honest gotchas:

- **Lots of `unsafe`.** Every FFI call is unsafe; lifetime mistakes are
  segfaults, not panics.
- **Crashes propagate.** A bug in the `.so` takes the engine down. There's
  no sandbox.
- **Versioning the boundary** is your job — both sides must agree on the
  Arrow C Data Interface version and on your own struct layouts.

A Rust-side macro can hide most of the ceremony for the common "scalar UDF
on Arrow arrays" case, leaving the user with normal-looking Rust.

## Sketch

```rust
#[derive(Clone)]
pub struct UdfDylib {
    name: Arc<String>,
    signature: Arc<Signature>,
    return_type: Arc<DataType>,
    udf: Arc<Container<UdfDylibInterface>>,
}

#[derive(WrapperApi)]
struct UdfDylibInterface {
    run: unsafe extern "C-unwind" fn(
        args_ptr: *mut FfiArraySchemaPair,
        args_len: usize,
        args_capacity: usize,
    ) -> FfiArrayResult,
}

#[derive(Clone, Debug)]
struct FfiArraySchemaPair(FFI_ArrowArray, FFI_ArrowSchema);

#[repr(C)]
pub struct FfiArrayResult(FFI_ArrowArray, FFI_ArrowSchema, bool); // bool: was the result valid
```

The shared library exposes a single C-compatible entry point:

```rust
#[no_mangle]
pub extern "C-unwind" fn run(
    args_ptr: *mut FfiArraySchemaPair,
    args_len: usize,
    args_capacity: usize,
) -> FfiArrayResult {
    // ...
}
```

Under the hood the implementation can be Rust, C, C++, Zig, or anything else
that produces a C-ABI-compatible shared library. If implemented in Rust, a
helper macro can take care of translating between FFI Arrow arrays and native
`arrow::array::ArrayRef` so the user just writes a normal Rust function.

## Similar / related topics

- **DataFusion native `ScalarUDF`** — the in-process, no-FFI path; fastest
  but requires recompilation.
- **SingleStore WASM UDFs** — sandboxed alternative, higher overhead.
- **Postgres `plpython3u` / `plperlu`** — untrusted procedural-language UDFs;
  same trust model (engine trusts the code), different boundary.
- **Apache Arrow C Data Interface** — the zero-copy spec this design rides on.
- **`extism`** — WASM plugin framework, useful if you want sandboxing on top
  of a similar idea.

## Internal links

- [[db/udf/README|UDF]] — section landing.
- [[db/formats/table_transfer_protocols/README|Table transfer protocols]] —
  Arrow Flight is the over-the-wire counterpart to Arrow C Data Interface.
- [[db/analytics/README|analytics]] — DataFusion and friends.

## Keywords

`#external-udfs` `#udf` `#ffi` `#arrow` `#datafusion` `#dlopen` `#c-abi` `#rust` `#zero-copy`

## References / raw notes

- `dlopen2` crate: <https://lib.rs/crates/dlopen2>
- `dlopen2_derive` crate: <https://lib.rs/crates/dlopen2_derive>
- Arrow FFI module: <https://docs.rs/arrow/latest/arrow/ffi/index.html>

The shared-library + Arrow-FFI approach avoids the Rust ABI instability
problem of "compile your UDF crate against the same DataFusion": the C ABI
boundary is stable across compilers and versions. Zero-copy is possible
because Arrow batches are passed as pointer pairs to externally-allocated
memory.

There is a lot of `unsafe` in this implementation, so it should be done with
extra care.
