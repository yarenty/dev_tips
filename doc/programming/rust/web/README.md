---
title: Rust web stack
keywords: [web, rust, http, websocket, wasm, hyper, axum]
status: reviewed
review_date: 2026/05/03
---

# Rust web stack

The Rust web ecosystem covers everything from low-level HTTP wire implementations to full-stack Wasm frontend frameworks. This section is organised by layer rather than alphabetically; for the broader networking landscape (gRPC, MQTT, NATS) see [[programming/rust/net/README|Rust net]], for messaging substrates [[messaging/README|Messaging]], for desktop/mobile UIs [[programming/rust/gui/README|Rust gui]], and for async-runtime details [[programming/rust/concurrency/README|Rust concurrency]].

## Decision shortcuts

| You want… | Reach for |
|---|---|
| An async HTTP server, modern default | `[[axum]]` |
| Opinionated, macro-driven framework | `[[rocket]]` |
| Minimal Express.js-style microframework | `[[feather]]` |
| An HTTP client, batteries included | `[[reqwest]]` |
| Sync HTTP client (no tokio, small binary) | `ureq` (no article — external link only) |
| Drop below a framework, control wire | `[[http]]` (hyper) |
| Quick "serve this directory" CLI | `[[hyperfs]]` (or `miniserve`) |
| Parse / scrape HTML | `[[html]]` (kuchiki / scraper) |
| WebSockets server | `axum`'s extractor → `tokio-tungstenite` (`[[tungstenite]]`) |
| WebSockets client (browser) | `gloo-net::websocket` (`[[gloo]]`) |
| Compile Rust to WebAssembly | `[[webassembly]]` (overview) |
| Run `*.wasm` standalone or as plugin | `[[wasmtime]]` |
| Browser bindings layer above `web-sys` | `[[gloo]]` |
| Rust frontend framework (React-style) | [[yew]] (mature) or [[programming/rust/gui/dioxus|dioxus]] (multi-target) |
| Trace / profile guest Wasm modules | `[[observability_on_wasm]]` |

## Articles

### HTTP servers / frameworks

- [Axum](axum.md) — Tokio-team's tower-based framework; the modern default.
- [Rocket](rocket.md) — opinionated, macro-driven framework by Sergio Benitez.
- [Feather](feather.md) — Express.js-inspired Rust microframework.

### HTTP clients

- [reqwest](reqwest.md) — ergonomic batteries-included async HTTP client.

### HTTP / low-level

- [hyper](http.md) — the foundational HTTP/1+2 implementation underneath the rest of the stack.
- [HyperFS](hyperfs.md) — single-binary static-file server (cf. `python -m http.server`).

### HTML parsing

- [kuchiki](html.md) — CSS-selector HTML/XML tree manipulation (with notes on `scraper` as the maintained successor).

### WebSockets

- [tungstenite](tungstenite.md) — the canonical WebSocket library (and its async sibling `tokio-tungstenite`).

### WebAssembly toolchain

- [WebAssembly (overview)](webassembly.md) — the Rust+Wasm landing page; toolchain map and target triples.
- [wasmtime](wasmtime.md) — Bytecode Alliance's standalone runtime; embed it for plugins.
- [gloo](gloo.md) — modular ergonomic wrappers over `web-sys`/`js-sys`.
- [Yew](yew.md) — the long-running React-style frontend framework.
- [Observability on Wasm](observability_on_wasm.md) — Dylibso's `observe-sdk` for tracing guest modules.

## Keywords

`#web` `#rust` `#http` `#websocket` `#wasm` `#hyper` `#axum`

## See also

- [[programming/rust/net/README|Rust net]] — gRPC, MQTT, NATS, tunnelling
- [[programming/rust/gui/README|Rust gui]] — desktop / mobile / Tauri / Dioxus
- [[programming/rust/concurrency/README|Rust concurrency]] — tokio, actors, streaming
- [[messaging/README|Messaging]] — Kafka, NATS, Redpanda, Pulsar, MQTT
- [[programming/rust/tooling/README|Rust tooling]] — `trunk`, `wasm-pack`, `cargo-component`
- [[programming/rust/plugins/README|Rust plugins]] — plugin systems often built on `wasmtime`
