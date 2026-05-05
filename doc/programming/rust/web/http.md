---
title: Hyper
main_link: https://github.com/tokio-rs/axum
keywords: [web, rust, programming, rocket, axum, crates, simple]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Hyper

**Main link:** <https://github.com/tokio-rs/axum>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#web` `#rust` `#programming` `#rocket` `#axum` `#crates` `#simple`

## TODO

- This file contains **6 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Hyper 

https://crates.io/crates/hyper


A fast and correct HTTP implementation for Rust.

- HTTP/1 and HTTP/2
- Asynchronous design
- Leading in performance
- Tested and correct
- Extensive production use
- Client and Server APIs

Low level!

# HyperFS

https://crates.io/crates/hyperfs

A simple HTTP server for static files.

HyperFS is a very simple http server that simply serves the files in the directory it is run from. It is primarily intended as a development tool similar to using python -m SimpleHTTPServer. But it is a single lightweight executable written in rust.

To be honest, part of the motivation was to just so I could try out rust.



# Rewquest

https://crates.io/crates/reqwest

An ergonomic, batteries-included HTTP Client for Rust.

- Plain bodies, JSON, urlencoded, multipart
- Customizable redirect policy
- HTTP Proxies
- HTTPS via system-native TLS (or optionally, rustls)
- Cookie Store
- WASM



# Rocket

https://rocket.rs/

Rocket is a web framework for Rust that makes it simple to write fast, secure web applications without sacrificing flexibility, usability, or type safety.




* Type Safe
From request to response Rocket ensures that your types mean something.

* Boilerplate Free
Spend your time writing code that really matters, and let Rocket generate the rest.

* Easy To Use
Simple, intuitive APIs make Rocket approachable, no matter your background.

* Extensible
Create your own first-class primitives that any Rocket application can use.


```rust
#[macro_use] extern crate rocket;

#[get("/hello/<name>/<age>")]
fn hello(name: &str, age: u8) -> String {
    format!("Hello, {} year old named {}!", age, name)
}

#[launch]
fn rocket() -> () {
    rocket::build().mount("/", routes![hello])
}
```


This is a complete Rocket application. It does exactly what you would expect. If you were to visit /hello/John/58, you’d see:

Hello, 58 year old named John!

If someone visits a path with an <age> that isn’t a u8, Rocket doesn’t blindly call hello. Instead, it tries other matching routes or returns a 404.



# Axum 

https://github.com/tokio-rs/axum

https://docs.rs/axum/latest/axum/

There is proposition to demote rocket and use Axum instead.
Axum is build by tokio team

https://tokio.rs/blog/2022-11-25-announcing-axum-0-6-0


No macros
No middleware - use tower / tonic / hyper / - easy integration.


Official features:

- Route requests to handlers with a macro-free API.
- Declaratively parse requests using extractors.
- Simple and predictable error handling model.
- Generate responses with minimal boilerplate.
- Take full advantage of the tower and tower-http ecosystem of middleware, services, and utilities.

https://medium.com/@lindblomdev/beginning-rust-by-exploring-a-very-basic-axum-web-api-in-detail-1f4c87e422e0

https://www.youtube.com/watch?v=XZtlD_m59sM  - full course 



# Tungstenite

https://crates.io/crates/tungstenite

Lightweight stream-based WebSocket implementation for Rust.

```rust
use std::net::TcpListener;
use std::thread::spawn;
use tungstenite::accept;

/// A WebSocket echo server
fn main () {
let server = TcpListener::bind("127.0.0.1:9001").unwrap();
for stream in server.incoming() {
spawn (move || {
let mut websocket = accept(stream.unwrap()).unwrap();
loop {
let msg = websocket.read().unwrap();

                // We do not want to send back ping/pong messages.
                if msg.is_binary() || msg.is_text() {
                    websocket.send(msg).unwrap();
                }
            }
        });
    }
}
```
Take a look at the examples section to see how to write a simple client/server.
