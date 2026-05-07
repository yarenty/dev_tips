---
title: hyper
main_link: https://hyper.rs/
keywords: [hyper, rust, http, http2, low-level, tokio]
status: reviewed
review_date: 2026/05/03
---

# hyper

**Main link:** <https://hyper.rs/>

## Summary

`hyper` is the foundational HTTP library for the Rust async ecosystem — a correct, performant, low-level implementation of HTTP/1, HTTP/2, and (via plugins) HTTP/3, with both client and server APIs. Maintained by Sean McArthur under the Tokio org. Almost every higher-level Rust HTTP tool — `axum`, `reqwest`, `tonic`, `warp`, `tower-http`, `rocket` (since 0.5) — is built on top of it. The 1.0 release (Nov 2023) finally stabilised the API after years on 0.14.

This article serves as the section's HTTP-stack overview; sibling articles cover the higher-level frameworks/clients split out from it (see Internal links).

## Insight

Use `hyper` directly when you need to build something *underneath* a framework (custom proxies, gateways, SDKs, low-overhead servers) or when you want full control over connection management, HTTP/2 settings, or upgrade flows. For application code, almost always reach for `[[axum]]` (server) or `[[reqwest]]` (client) instead — they hide the wiring without hiding the underlying types when you need them.

The wider stack to keep straight:

- **`http`** — types only (`Request`, `Response`, `Method`, `StatusCode`, `HeaderMap`). No I/O. Used by everyone.
- **`http-body`** — the streaming-body trait the rest of the ecosystem implements.
- **`hyper`** — actual HTTP/1+2 wire implementation; client + server.
- **`tower` / `tower-http`** — request/response middleware traits and ready-made layers (timeouts, tracing, CORS, compression).
- **`hyper-util`** — connection pooling, executors, "bring your own runtime" helpers post-1.0.

Gotchas:

- The 1.0 split moved a lot of "batteries" (legacy `Client`, `Server`) out of `hyper` itself into `hyper-util`. Many older snippets won't compile against 1.x.
- HTTP/2 "smuggling" / connection-preface concerns make picking the right TLS+ALPN setup important if you're terminating directly.
- For HTTP/3 you need `hyper` 1.x + an `h3-quinn` adapter; not as turn-key as HTTP/2 yet.

## Similar / related topics

- **actix-http** — actix-web's bespoke HTTP layer.
- **`http` crate** — type definitions every Rust HTTP tool shares.
- **h2 / quinn / h3** — protocol-specific crates `hyper` builds on.
- **monoio-http / glommio** — alternative low-level HTTP for thread-per-core runtimes.
- **`ureq`** — sync, no-tokio HTTP client; small dependency footprint.

## Internal links
- [[axum]] — the recommended high-level server
- [[rocket]] — opinionated framework also on hyper since 0.5
- [[reqwest]] — the high-level client
- [[hyperfs]] — single-file static-server example built on hyper
- [[tungstenite]] — WebSockets on top of an HTTP upgrade
- [[tokio]] — the async runtime hyper integrates with
- [[grpc]] — `tonic`, gRPC over HTTP/2 via hyper

## Keywords

`#hyper` `#http` `#http2` `#rust` `#tokio` `#low-level` `#networking`

## References / raw notes

- Site: <https://hyper.rs/>
- Crate: <https://crates.io/crates/hyper>
- Repo: <https://github.com/hyperium/hyper>

From the project page:

> A fast and correct HTTP implementation for Rust.
>
> - HTTP/1 and HTTP/2
> - Asynchronous design
> - Leading in performance
> - Tested and correct
> - Extensive production use
> - Client and Server APIs

Low-level — almost always consumed via a higher-level wrapper. Sibling articles split out from this one cover those wrappers: `[[axum]]`, `[[rocket]]`, `[[reqwest]]`, `[[tungstenite]]`, `[[hyperfs]]`.
