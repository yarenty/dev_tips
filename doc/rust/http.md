# Hyper 

https://crates.io/crates/hyper


A fast and correct HTTP implementation for Rust.

- HTTP/1 and HTTP/2
- Asynchronous design
- Leading in performance
- Tested and correct
- Extensive production use
- Client and Server APIs



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
No macros
No middleware - use tower / tonic / hyper / - easy integration.


Official features:

- Route requests to handlers with a macro-free API.
- Declaratively parse requests using extractors.
- Simple and predictable error handling model.
- Generate responses with minimal boilerplate.
- Take full advantage of the tower and tower-http ecosystem of middleware, services, and utilities.