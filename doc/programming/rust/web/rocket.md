---
title: Rocket
main_link: https://rocket.rs/
keywords: [rocket, rust, web-framework, macros, type-safe]
status: reviewed
---

# Rocket

**Main link:** <https://rocket.rs/>

## Summary

Rocket is an opinionated, macro-driven web framework for Rust started in 2016 by Sergio Benitez (UCSD/Stanford). It uses attribute macros (`#[get("/path/<name>")]`) to bake routing, type-checked path/query parameters, and request guards directly into handler signatures, so a lot of plumbing other frameworks expose as builder calls becomes compile-time checked. The 0.5 release (Nov 2023) finally moved it from a custom synchronous runtime to fully async on top of `tokio` + `hyper`, ending the long "use 0.4 sync or nightly-only async-preview" awkwardness.

## Insight

Reach for Rocket when you want the *most* batteries-included DX in the Rust web space — type-safe routing with friendly error messages, built-in request guards, fairings (Rocket's middleware), templating, forms, JSON, and managed state all configured by convention. Trade-off: it's heavier on macros and conventions than Axum, has a smaller ecosystem of third-party middleware, and historically lagged on async/stable-Rust support. The Rust web community largely settled on `[[axum]]` as the default after 2022, so most production guidance points there now; Rocket remains a good choice for small services, teaching, and projects that value DX over composability.

Things to know:

- **0.4 → 0.5 migration is not trivial.** Async handler signatures, the new `Build`/`Ignite`/`Orbit` lifecycle, and async fairings broke most 0.4 code. Don't follow pre-2024 tutorials.
- **Single-author bus factor.** Most of the framework's evolution has historically come from Sergio Benitez; cadence has slowed.
- **Conventions over composability.** Rocket prefers its own abstractions (`Fairing`, `Responder`, `FromRequest`) over `tower`'s `Service`/`Layer`, so middleware written for Axum/tonic doesn't drop in.

## Similar / related topics

- **axum** — the modern default; less magic, more `tower` composability.
- **actix-web** — performance leader; bespoke runtime model.
- **poem** — extractor-style framework with built-in OpenAPI.
- **warp** — filter combinators; effectively unmaintained.
- **Loco** — "Rails for Rust" built on top of Axum; even more opinionated than Rocket.

## Internal links
<!-- reviewed -->
- [[axum]] — the modern alternative most new projects pick
- [[http]] — the underlying `hyper` HTTP layer
- [[reqwest]] — natural HTTP-client pairing for testing Rocket services
- [[tokio]] — the async runtime since 0.5
- [[loco_rust_on_rails|Loco]] — Rails-shaped Rust framework, also opinionated

## Keywords

`#rocket` `#web` `#rust` `#macros` `#type-safe` `#framework`

## References / raw notes

- Site: <https://rocket.rs/>
- Repo: <https://github.com/rwf2/Rocket>
- Author: Sergio Benitez (UCSD); now under the RWF2 org.

Selling points (from the site):

- **Type Safe.** From request to response, Rocket ensures your types mean something.
- **Boilerplate Free.** Spend your time writing code that really matters; let Rocket generate the rest.
- **Easy To Use.** Simple, intuitive APIs; approachable regardless of background.
- **Extensible.** First-class primitives any Rocket app can use.

Hello-world (note: this is the 0.5 async form):

```rust
#[macro_use] extern crate rocket;

#[get("/hello/<name>/<age>")]
fn hello(name: &str, age: u8) -> String {
    format!("Hello, {} year old named {}!", age, name)
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![hello])
}
```

Visiting `/hello/John/58` returns `Hello, 58 year old named John!`. If `<age>` doesn't parse as `u8`, Rocket tries other matching routes or returns a 404 — the type system, not a runtime guard, enforces the contract.
