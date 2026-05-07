---
title: Axum
main_link: https://github.com/tokio-rs/axum
keywords: [axum, rust, tokio, tower, hyper, web-framework, middleware]
status: reviewed
review_date: 2026/05/03
---

# Axum

**Main link:** <https://github.com/tokio-rs/axum>

## Summary

Axum is the Tokio team's web framework, built directly on `hyper` and the `tower`/`tower-http` middleware ecosystem. It's macro-free at the routing layer, leans hard on Rust's type system for request/response ergonomics ("extractors"), and composes middleware as `tower::Service` layers — so anything that works for `tonic` (gRPC) or generic `tower` clients/servers also works for Axum. Officially announced in 2021 and stable since 0.6 (late 2022); the current line is 0.7/0.8 with breaking-but-small migrations between minors.

## Insight

Reach for Axum when you want a modern async Rust HTTP server with a very thin abstraction over `hyper`, first-class support for streams/WebSockets, and the freedom to drop down to raw `tower` for cross-cutting concerns (timeouts, retries, rate limits, tracing, CORS). It's now the de-facto default in the Rust web ecosystem — actix-web is still faster on synthetic benchmarks but has a more bespoke design; warp (its conceptual predecessor by the same author, David Pedersen) is effectively unmaintained; rocket has the most opinionated DX but a much smaller ecosystem; poem is a close cousin with a similar extractor style.

Gotchas worth knowing up front:

- **Extractor ordering matters.** Body-consuming extractors (`Json`, `Form`, `Bytes`) must be the *last* argument in a handler — only one extractor may consume the body.
- **Breaking minor releases.** Each Axum minor (0.5 → 0.6 → 0.7 → 0.8) has reshuffled the `Router` / `IntoResponse` / state types. Pin tightly and read the changelog.
- **State sharing.** Use `Router::with_state(...)` (typed) rather than the older extension-map `Extension<T>`; the typed approach catches missing state at compile time.
- **Tower learning curve.** The middleware story is "use tower", which is powerful but unfamiliar — `tower::ServiceBuilder` and the `Layer`/`Service` traits are the real API surface.

## Similar / related topics

- **actix-web** — the long-standing performance leader; uses its own actor-flavoured runtime model, more bespoke than Axum's tower-based design.
- **rocket** — opinionated, macro-heavy framework by Sergio Benitez; see `[[rocket]]`.
- **poem** — Axum-style extractor framework with built-in OpenAPI; smaller community.
- **warp** — Pedersen's earlier filter-combinator framework; superseded by Axum.
- **salvo** — newer entrant aiming at simplicity + middleware-as-handler ergonomics.

## Internal links
- [[http]] — the `hyper` crate that Axum sits on top of
- [[rocket]] — the opinionated alternative
- [[tungstenite]] — Axum's WebSocket extractor delegates to `tokio-tungstenite`
- [[reqwest]] — natural HTTP-client pairing for Axum services
- [[tokio]] — the runtime Axum requires
- [[grpc]] — `tonic`, the same team's gRPC framework, also tower-based

## Keywords

`#axum` `#web` `#rust` `#tokio` `#tower` `#hyper` `#middleware` `#api`

## References / raw notes

Official:

- Repo: <https://github.com/tokio-rs/axum>
- Docs: <https://docs.rs/axum/latest/axum/>
- 0.6 announcement: <https://tokio.rs/blog/2022-11-25-announcing-axum-0-6-0>

Selling points (from the README):

- Route requests to handlers with a macro-free API.
- Declaratively parse requests using extractors.
- Simple and predictable error-handling model.
- Generate responses with minimal boilerplate.
- Take full advantage of the `tower` and `tower-http` ecosystem of middleware, services, and utilities.

Walk-throughs:

- Beginner Axum web API, line by line: <https://medium.com/@lindblomdev/beginning-rust-by-exploring-a-very-basic-axum-web-api-in-detail-1f4c87e422e0>
- Full-course video: <https://www.youtube.com/watch?v=XZtlD_m59sM>

Author: David Pedersen (also author of `warp`); now maintained by the Tokio org.
