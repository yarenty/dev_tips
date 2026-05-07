---
title: Rust interop (FFI / language bridges)
keywords: [interop, ffi, python, java, dotnet, pyo3, jni, csbindgen]
status: reviewed
---

# Rust interop (FFI / language bridges)

How a Rust crate talks to — and is talked to by — other languages. The
section is organised by *peer language*: Rust ↔ Python (the largest
ecosystem by far), Rust ↔ JVM, Rust ↔ .NET. The WebAssembly story is
covered separately under [[../web/wasmtime|wasmtime]] and
[[../web/webassembly|webassembly]] because it doesn't slot cleanly into
"a peer language" — Wasm is more like a portable target ABI.

## Articles

### Rust ↔ Python

The most-trodden path. Default stack: [[pyo3|PyO3]] for the FFI layer +
[[maturin]] for the wheel build/publish pipeline. Real-world consumers
include Polars, `pydantic-core`, Ruff, `cryptography`, Tokenizers.

- [[pyo3|PyO3]] — canonical Rust ↔ CPython binding crate.
- [[maturin]] — build PyO3 / cffi / uniffi crates as Python wheels.
- [[to_python]] — recipe: "I have a Rust crate, how do I publish it
  to PyPI?"
- [[ballista_py]] — worked example: PyO3 + maturin bindings for
  distributed DataFusion.
- [[programming/rust/interop/python|python (PUFF)]] — the inverse
  direction (embed CPython inside a Rust runtime); also serves as the
  historical Python-interop overview parent.

### Rust ↔ JVM

- [[to_java]] — JNI (`jni-rs`, `jnix`, `robusta`), `j4rs` for the
  reverse direction, `duchess` for typed bindings, Java 22 FFM API as
  the modern replacement for JNI, plus the QuestDB case study.

### Rust ↔ .NET

- [[to_net]] — `csbindgen` (Cysharp's de-facto Rust → C# binding
  generator), `netcorehost` for hosting CoreCLR from Rust, and
  FractalFir's experimental `rustc_codegen_clr` (Rust source → .NET
  assemblies, no FFI).

## When to reach for what

| Situation | Reach for |
|-----------|-----------|
| Expose a Rust crate to Python | [[pyo3]] + [[maturin]] |
| Embed CPython in a Rust binary | [[pyo3]] embedding mode (or `pyo3-async-runtimes`) |
| Expose a Rust crate to C# | `csbindgen` (see [[to_net]]) |
| Compile Rust to a .NET assembly | `rustc_codegen_clr` (experimental, [[to_net]]) |
| Expose a Rust crate to Java (legacy JNI) | [[to_java]] (`jni` crate) |
| Expose a Rust crate to Java (modern) | Java 22 FFM API + `cdylib` (see [[to_java]]) |
| Drive a JVM from Rust | `j4rs` (see [[to_java]]) |
| Cross-language bindings (Py + Kotlin + Swift) | `uniffi-rs` |
| Run Rust as portable bytecode embedded in any host | [[../web/wasmtime|wasmtime]] |

## See also

- [[../web/wasmtime|wasmtime]] / [[../web/webassembly|webassembly]] —
  Wasm-as-FFI: compile Rust to a portable bytecode and embed it from any
  host language.
- [[../io/README]] — adjacent topic: bytes-on-the-wire, Arrow IPC, and
  serde, which often bound interop's actual cost.
- [[../core/reflection]] — runtime introspection in Rust (relevant to
  generators like `csbindgen` / `duchess` / `uniffi`).
- [[../README]] — Rust language landing.

## Keywords

`#interop` `#ffi` `#pyo3` `#maturin` `#jni` `#csbindgen` `#wasm`
