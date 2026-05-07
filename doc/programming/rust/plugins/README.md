---
title: Rust plugin substrates
keywords: [plugins, wasm, dynamic-loading, scripting, extism, mlua, rhai]
status: reviewed
---

# Rust plugin substrates

A stub landing page. There are no articles in this section yet — but the
problem space ("how do I let users extend my Rust application without
recompiling it?") is well worth knowing the answer to. The honest
answers in 2025 split into three families.

## The three plugin substrates

### 1. WebAssembly (the modern recommendation)

Compile plugin code (Rust, C, Go, AssemblyScript, etc.) to a `.wasm`
module; load it from your Rust host with a Wasm runtime. Sandbox is
real, capability-based, cross-platform, and language-agnostic.

- **[`wasmtime`](https://github.com/bytecodealliance/wasmtime)** — the
  Bytecode Alliance reference runtime; covered in
  [[../web/wasmtime|wasmtime]].
- **[`wasmer`](https://github.com/wasmerio/wasmer)** — alternative
  runtime, more business-features-focused.
- **[`extism`](https://extism.org/)** — opinionated framework
  *on top of* a Wasm runtime that gives you a higher-level "PDK"
  (Plugin Development Kit) per language plus host SDKs in a dozen
  others. Closest thing to a "plugin platform" in 2025.

### 2. Dynamic libraries (`dlopen` / `LoadLibrary`)

Plugins are compiled `.so` / `.dylib` / `.dll` files; the host loads
them with `libloading`. No sandbox, no ABI stability across compilers
— everything must agree on `extern "C"` signatures.

- **[`libloading`](https://github.com/nagisa/rust_libloading)** — the
  canonical safe-ish wrapper.
- **[`dlopen2`](https://github.com/OpenByteDev/dlopen2)** — derive-
  driven sugar on top.
- **[`abi_stable`](https://github.com/rodrimati1992/abi_stable_crates)**
  — a Rust-to-Rust stable-ABI layer; lets you cross the `cdylib`
  boundary with traits and Vec/String, at the cost of
  ceremony.

### 3. Embedded scripting languages

Embed an interpreter in your Rust binary; users write plugins in a
scripting language with much less ceremony than C/Wasm/Rust.

- **[`mlua`](https://github.com/mlua-rs/mlua)** — bindings to Lua /
  LuaJIT / Luau. The most popular game-modding choice.
- **[`rhai`](https://rhai.rs/)** — pure-Rust embedded scripting
  language (Rust-shaped syntax). Easy to embed, sandboxed by
  construction.
- **[`boa`](https://boajs.dev/)** — pure-Rust JavaScript engine.
- **[`pyo3` embedding mode](https://pyo3.rs/)** — embed CPython itself
  (see [[../interop/python|interop/python (PUFF)]]).
- **`deno_core`** — embed the Deno V8-based JS/TS runtime.

## When to reach for which

| Constraint                                              | Reach for                |
|---------------------------------------------------------|--------------------------|
| Untrusted plugin authors / strong sandbox required      | Wasm (`wasmtime` / `extism`) |
| Plugins are first-party, performance-critical, trusted  | `libloading` + `abi_stable`  |
| Plugins are user-written game/UX tweaks                 | `rhai` or `mlua`        |
| Plugins are "let power users automate this app"         | `rhai`                  |
| You need polyglot plugin authoring                      | `extism` (or raw Wasm)  |
| You want existing Python ecosystem available            | [[../interop/python|interop/python]] (`pyo3` embed) |

## See also

- [[../web/wasmtime|wasmtime]] — the canonical Wasm runtime in Rust.
- [[../web/webassembly|webassembly]] — broader Wasm framing.
- [[../core/reflection]] — runtime introspection, often desired
  alongside plugin systems.
- [[../interop/README]] — sibling section: FFI / language bridges
  (overlaps with the dynamic-library substrate above).
- [[../README]] — Rust language landing.

## Keywords

`#plugins` `#wasm` `#extism` `#libloading` `#rhai` `#mlua` `#scripting`
