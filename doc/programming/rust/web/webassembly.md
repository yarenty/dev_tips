---
title: WebAssembly (Rust overview)
main_link: https://rustwasm.github.io/docs/book/
keywords: [webassembly, wasm, rust, wasm-bindgen, wasm-pack, trunk, browser]
status: reviewed
---

# WebAssembly (Rust overview)

**Main link:** <https://rustwasm.github.io/docs/book/>

## Summary

WebAssembly (Wasm) is a portable, sandboxed bytecode format originally designed for the browser and now widely used as a standalone module format on servers, edge platforms, and embedded systems. Rust has been a first-class Wasm producer since 2017, with the rust-lang `wasm-bindgen` + `wasm-pack` toolchain on the browser side and the Bytecode Alliance's `wasmtime` + `wit-bindgen` toolchain on the standalone-runtime side. This article is the section's landing page; the toolchain pieces have their own articles (see Internal links).

## Insight

The Rust WebAssembly story splits cleanly along two axes: **target triple** and **runtime**.

- **`wasm32-unknown-unknown`** + `wasm-bindgen` — for browsers. Generates JS glue, calls `web-sys`/`js-sys` for browser APIs.
- **`wasm32-wasi(p1|p2)`** — for non-browser runtimes (`wasmtime`, `wasmer`). Has POSIX-flavoured syscalls (filesystem, env, clocks). The Component Model (`wasi-p2`) is the long-term target.
- **`wasm32-unknown-emscripten`** — legacy; almost never the right choice for new Rust code.

Browser-side toolchain (in roughly the order you'd reach for them):

| Tool | Role |
|---|---|
| `wasm-bindgen` | Bridge between Rust types and JS values (the macro). |
| `web-sys` / `js-sys` | Auto-generated bindings for every browser/JS API. |
| `gloo` | Higher-level, ergonomic wrappers over `web-sys`. See `[[gloo]]`. |
| `wasm-pack` | Builds a publishable npm package. |
| `trunk` | Bundler/dev-server tailored to Rust+Wasm SPAs. |
| `yew` / `leptos` / `dioxus` / `sycamore` | Frontend frameworks built on the above. |
| `wasm-opt` (binaryen) | Post-build size/perf optimiser; non-optional for prod. |

Server/edge toolchain:

| Tool | Role |
|---|---|
| `wasmtime` | Bytecode Alliance reference runtime. See `[[wasmtime]]`. |
| `wasmer` | Alternative runtime; broader language SDK story. |
| `wasmedge` | CNCF runtime; AI/edge focus. |
| `wit-bindgen` | Bindgen for the Component Model interfaces (`*.wit`). |
| `cargo-component` | Cargo subcommand that builds Component-Model components. |
| `spin` (Fermyon) | Higher-level Wasm-as-microservices framework on wasmtime. |

Where it actually matters in 2025:

- **Plugin systems.** Embedding `wasmtime` is the new standard way to ship a sandboxed plugin/extension story (used by Zellij, Lapce, OpenObserve, Envoy, Istio).
- **Edge compute.** Cloudflare Workers, Fastly Compute@Edge, Vercel Edge — all ship Wasm.
- **Frontend.** Real but niche; React/Vue still dominate. Yew/Leptos/Dioxus carve out a space for Rust-only teams.
- **Embedded UI.** Tauri / Dioxus ship Wasm in a webview.

Gotchas:

- **Binary size.** Default release Rust + Wasm is huge; `wasm-opt -Oz` + `panic = "abort"` + `lto = true` + `strip = true` are the table stakes.
- **Threading.** Cross-origin isolation (`SharedArrayBuffer`) and `wasm-bindgen-rayon` are required; not free.
- **Async in browsers.** Use `wasm-bindgen-futures` to bridge `Future` ↔ JS Promise; tokio doesn't run in browsers.
- **WASI version churn.** `wasi-p1` (legacy) vs `wasi-p2` (Component Model) is a real fork in the road; pick deliberately.

## Similar / related topics

- **wasm-bindgen / wasm-pack** — the rust-lang Wasm-WG toolchain.
- **Component Model** — the future module ABI; superset of WASI p1.
- **emscripten** — the C/C++ Wasm toolchain; less relevant for Rust.
- **AssemblyScript** — TypeScript-flavoured language that compiles to Wasm.
- **PyScript / Pyodide** — Python-on-Wasm; orthogonal but adjacent.

## Internal links
<!-- reviewed -->
- [[wasmtime]] — the standalone Wasm runtime
- [[yew]] — frontend framework on wasm-bindgen
- [[gloo]] — higher-level browser bindings
- [[observability_on_wasm]] — tracing/logging from inside Wasm modules
- [[dioxus]] — alternative React-style Rust GUI / Wasm framework
- [[programming/rust/tooling/README|tooling]] — for `wasm-opt`, `cargo-component`, `trunk`

## Keywords

`#webassembly` `#wasm` `#rust` `#wasm-bindgen` `#wasm-pack` `#trunk` `#browser` `#bytecodealliance`

## References / raw notes

- The Rust + WebAssembly book: <https://rustwasm.github.io/docs/book/>
- WebAssembly proposals / status / meetings: <https://github.com/WebAssembly>
- Bytecode Alliance (wasmtime, tools, runtime, bindgen): <https://github.com/bytecodealliance>
- The Rust Wasm Working Group: <https://github.com/rustwasm>
- Component Model: <https://component-model.bytecodealliance.org/>

A useful video intro (from the original notes): <https://www.youtube.com/watch?v=DkpNqcfuPOM>

Sibling articles split out from this one cover the individual pieces of the toolchain — see the Internal links above.
