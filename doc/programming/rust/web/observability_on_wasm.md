---
title: Observability on Wasm (Dylibso Observe SDK)
main_link: https://github.com/dylibso/observe-sdk
keywords: [observability, wasm, tracing, dylibso, otel, observe-sdk, rust]
status: reviewed
---

# Observability on Wasm (Dylibso Observe SDK)

**Main link:** <https://github.com/dylibso/observe-sdk>

## Summary

Dylibso's `observe-sdk` is a host-side library (with bindings for Rust, Go, Node, Python and others) that automatically instruments a guest WebAssembly module's function-entry/exit timing and emits the resulting traces to OpenTelemetry, Datadog, Honeycomb, Lightstep, or stdout. It does this by recompiling the module with bytecode-level adapter shims, so the guest doesn't need to be modified or even rebuilt with special flags. Aimed at the "I'm running untrusted Wasm plugins and need to know what they're doing" use case.

## Insight

Reach for `observe-sdk` when:

- You're hosting Wasm modules (via `wasmtime`/`wasmer`) as plugins or tenant code and want per-function spans and timing without trusting the guest to instrument itself.
- You want OpenTelemetry-compatible traces from a Wasm extension surface (Envoy/Istio Proxy-Wasm, Shopify Functions, Spin apps, etc.).
- You're debugging a slow guest module and `wasmtime`'s built-in `--profile` isn't fine-grained enough.

Things to know:

- **Adapter mechanism.** `observe-sdk` uses Dylibso's `Coraza`/`extism`-adjacent toolchain to inject instrumentation; the guest binary is rewritten on load. Doesn't need source access.
- **Overhead is real.** Per-function-entry tracing isn't free; sample or filter for production.
- **Vendor independence.** Dylibso is the sponsor; the SDK itself is permissively licensed and the OTLP path means you're not locked in.
- **Scope is narrow.** This is *trace* observability for Wasm — for metrics, you still want the host's normal Prometheus/OTel pipeline; for logs, the guest needs to emit them via `wasi:logging` or a host import.

Alternatives / adjacent tools:

- **Wasmtime's built-in profiling** (`--profile=jitdump|vtune|perfmap`) — fine-grained but host-side only.
- **OpenTelemetry SDK in the guest** — works if you control the guest source.
- **`wasm-tools profile` / `wasi-cli` instrumentation hooks** — newer, spec-track approaches via the Component Model.

This article disambiguates from the duplicate `programming/rust/tooling/observability_on_wasm.md` (same content, different parent split). The web/ copy is the canonical home for this topic since it's tightly tied to the Wasm runtime story.

## Similar / related topics

- **Extism** — Dylibso's broader "Wasm as a plugin runtime" framework; same family of tools.
- **OpenTelemetry** — the open trace/metrics/logs standard `observe-sdk` exports to.
- **Wasmtime profiling** (`--profile`) — host-side built-in alternative.
- **Proxy-Wasm + Envoy stats** — built-in metrics for the Envoy filter use case.

## Internal links
<!-- reviewed -->
- [[wasmtime]] — typical host runtime for the modules being observed
- [[webassembly]] — section overview
- [[observability/README|observability]] — broader observability landing page
- [[programming/rust/tooling/observability_on_wasm|tooling/observability_on_wasm]] — the duplicate stub, kept for cross-link compatibility

## Keywords

`#observability` `#wasm` `#tracing` `#dylibso` `#otel` `#observe-sdk` `#rust`

## References / raw notes

- Repo: <https://github.com/dylibso/observe-sdk>
- Rust adapter: <https://github.com/dylibso/observe-sdk/tree/main/rust>
- Dylibso (sponsor): <https://dylibso.com/>
- Extism (sister project): <https://extism.org/>
