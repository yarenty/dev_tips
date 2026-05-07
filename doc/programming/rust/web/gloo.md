---
title: gloo
main_link: https://gloo-rs.web.app/
keywords: [gloo, wasm, browser, web-sys, rust, toolkit]
status: reviewed
review_date: 2026/05/03
---

# gloo

**Main link:** <https://gloo-rs.web.app/>

## Summary

`gloo` is the Rust Wasm Working Group's modular toolkit of *ergonomic* wrappers around browser APIs — a higher-level layer over `web-sys` and `js-sys`. Where `web-sys` gives you literal one-to-one bindings to every WebIDL interface (and the `unsafe`-feeling `Result<JsValue, JsValue>` calling convention), `gloo` exposes idiomatic Rust types: `gloo_storage::LocalStorage::set`, `gloo_console::log!`, `gloo_net::http::Request::get`, `gloo_timers::callback::Timeout::new`, etc. Each capability is its own crate so you only pay for what you use.

## Insight

Reach for gloo whenever you find yourself writing `web_sys::window().unwrap().local_storage().unwrap().unwrap().set_item(...)` boilerplate. The named-by-domain crates currently include:

| Crate | Replaces |
|---|---|
| `gloo-storage` | localStorage / sessionStorage with serde |
| `gloo-console` | `console.log` etc. with a `log!` macro |
| `gloo-events` | typed event listeners (auto-detach on drop) |
| `gloo-timers` | `setTimeout` / `setInterval`, sync + future flavours |
| `gloo-net` | `fetch` and `WebSocket` |
| `gloo-file` | `File`, `Blob`, `FileReader` |
| `gloo-history` | History/Location/router primitives |
| `gloo-utils` | DOM helpers (window, document, format helpers) |
| `gloo-worker` | typed Web Workers |
| `gloo-dialogs` | `alert`, `confirm`, `prompt` |
| `gloo-render` | `requestAnimationFrame` as a Future |

When *not* to use it:

- A framework (Yew, Leptos, Dioxus) usually re-exports or re-implements the bits it needs; check there first.
- For one-off browser API access where you only need a single call, raw `web-sys` may be less code than pulling in a gloo crate.

Status note: `gloo` is "low-priority maintenance" by its own admission — releases are infrequent. It still works fine but doesn't track new browser APIs aggressively. For HTTP, `reqwest` (with `wasm32-unknown-unknown` target) often makes more sense in 2025 than `gloo-net`.

## Similar / related topics

- **`web-sys` / `js-sys`** — the underlying auto-generated browser/JS bindings.
- **`wasm-bindgen`** — the macro layer that everything in this stack is built on.
- **`reqwest`** (wasm target) — for HTTP, often nicer than `gloo-net`.
- **`yew-hooks` / `leptos-use`** — framework-specific equivalents bundled with each frontend framework.

## Internal links
- [[webassembly]] — section overview
- [[yew]] — frontend framework that pairs naturally with gloo
- [[reqwest]] — alternative for HTTP in WASM
- [[tungstenite]] — server-side WebSocket counterpart
- [[dioxus]] — alternative WASM frontend framework

## Keywords

`#gloo` `#wasm` `#browser` `#web-sys` `#rust` `#toolkit` `#frontend`

## References / raw notes

- Site: <https://gloo-rs.web.app/>
- Crate: <https://crates.io/crates/gloo>
- Repo: <https://github.com/rustwasm/gloo>

> A toolkit for building fast, reliable Web applications and libraries with Rust and Wasm.
>
> Gloo is a collection of libraries; those libraries provide ergonomic Rust wrappers for browser APIs. `web-sys`/`js-sys` are very difficult/inconvenient to use directly so gloo provides wrappers around the raw bindings, making it easier to consume those APIs. This is why it is called a "toolkit" rather than a "library" or "framework".
