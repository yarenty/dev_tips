---
title: wasmtime
main_link: https://wasmtime.dev/
keywords: [wasmtime, wasm, runtime, bytecode-alliance, cranelift, wasi, component-model]
status: reviewed
---

# wasmtime

**Main link:** <https://wasmtime.dev/>

## Summary

`wasmtime` is the Bytecode Alliance's reference standalone WebAssembly runtime — a fast, secure, embeddable Wasm + WASI engine written in Rust, JIT-compiled via `cranelift` (with optional AOT). It ships both as a CLI (`wasmtime run app.wasm`) and as an embedding library (`wasmtime` crate) that you link into your own Rust/C/Python/Go/.NET host. It's the canonical implementation that drives WASI standardisation and the Component Model. The headline use cases are: plugin host inside a Rust app (Zellij, Lapce, OpenObserve, Envoy/Istio Proxy-Wasm, Shopify, Fermyon Spin), edge / serverless functions, and as a CLI for running standalone `*.wasm` modules.

## Insight

Pick wasmtime over alternatives when you want the most actively-developed, spec-tracking runtime with the strongest "embed me in your own app" story:

- **wasmtime** — Bytecode Alliance reference; tracks WASI/Component Model first; permissive Apache-2.0; default for new projects.
- **wasmer** — broader language SDK story (Python, Ruby, PHP wrappers), pre-compiled package format (`.wasmer`); friendlier CLI; some divergence on Component Model timing.
- **wasmedge** — CNCF runtime; AI/edge focus, includes ML extensions and HTTP server bindings.
- **In-browser engines** (V8/Liftoff/TurboFan, SpiderMonkey, JSC) — the Wasm runtime in any browser; not relevant for standalone use.

Things to know:

- **Cranelift JIT** is the default; AOT (`wasmtime compile`) produces `.cwasm` artifacts that skip compilation at startup — important for serverless cold starts.
- **Resource limits.** `Engine::new` + `Config` lets you cap fuel (instruction count), memory, and stack — actually use these when hosting untrusted code.
- **Component Model** (`wasmtime` 14+) is the way to compose modules with typed interfaces (`*.wit`); `wasmtime serve` lets you run a `wasi:http` component as an HTTP server.
- **Async vs sync embed.** Pick at `Config::async_support(true)` time; can't mix per-instance.
- **Epoch-based interruption** is the modern way to time-out a runaway guest; older `Store::set_fuel` works too but is more invasive.

## Similar / related topics

- **wasmer** — alternative embeddable runtime; broader host-language SDKs.
- **wasmedge** — CNCF runtime with ML/edge extensions.
- **WAMR** (WebAssembly Micro Runtime) — Bytecode Alliance's interpreter/AOT for embedded targets.
- **wazero** — pure-Go embeddable runtime, no CGO.
- **`spin` (Fermyon)** — higher-level "Wasm microservices" framework on top of wasmtime.

## Internal links
<!-- reviewed -->
- [[webassembly]] — section overview
- [[observability_on_wasm]] — tracing/observability for guest modules
- [[programming/rust/plugins/README|plugins]] — embedding wasmtime as a plugin host
- [[programming/rust/tooling/README|tooling]] — `cargo-component`, `wit-bindgen`, `wasm-tools`

## Keywords

`#wasmtime` `#wasm` `#runtime` `#bytecode-alliance` `#cranelift` `#wasi` `#component-model`

## References / raw notes

- Site: <https://wasmtime.dev/>
- Crate: <https://crates.io/crates/wasmtime>
- Repo: <https://github.com/bytecodealliance/wasmtime>
- Bytecode Alliance: <https://bytecodealliance.org/>
- Component Model docs: <https://component-model.bytecodealliance.org/>

CLI smoke test:

```sh
cargo install wasmtime-cli
wasmtime run hello.wasm
wasmtime compile hello.wasm    # AOT to hello.cwasm
```

Minimal embedding (sync):

```rust
use wasmtime::{Engine, Module, Store, Instance};

let engine = Engine::default();
let module = Module::from_file(&engine, "add.wasm")?;
let mut store = Store::new(&engine, ());
let instance = Instance::new(&mut store, &module, &[])?;
let add = instance.get_typed_func::<(i32, i32), i32>(&mut store, "add")?;
println!("{}", add.call(&mut store, (2, 3))?);
```

Note: there's no separate sibling article on the Bytecode Alliance host stack — this article also serves that role.
