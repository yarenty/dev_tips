---
title: tungstenite
main_link: https://crates.io/crates/tungstenite
keywords: [tungstenite, websocket, rust, tokio, async]
status: reviewed
review_date: 2026/05/03
---

# tungstenite

**Main link:** <https://crates.io/crates/tungstenite>

## Summary

`tungstenite` is the canonical WebSocket implementation for Rust — a small, blocking, stream-based RFC-6455 client/server. The async wrapper `tokio-tungstenite` is the version most code actually depends on, and it's what backs `axum`'s `WebSocketUpgrade` extractor and `reqwest`'s WebSocket support. Maintained by the snapview team; quietly does the work for most of the Rust WebSocket ecosystem.

## Insight

The split is the part everyone gets wrong on first encounter:

- **`tungstenite`** — sync, blocking. Useful for tiny tools, tests, or `std::thread`-per-connection servers. Almost no production server you write today should use it directly.
- **`tokio-tungstenite`** — the async wrapper; what you actually want.
- Most frameworks (`axum`, `warp`, `actix-web`'s `actix-ws`) ship a higher-level extractor that hides `tokio-tungstenite` behind a friendlier type — use the framework helper unless you specifically need the lower level.

When to reach for `tokio-tungstenite` *directly* (vs. via Axum):

- You're writing a *client* (Axum is server-side).
- You need a standalone WebSocket server with no HTTP routing.
- You're building a custom upgrade flow / proxy.

Comparisons:

- **`fastwebsockets`** (Cloudflare/Deno team) — claims much higher throughput than tungstenite, used in Deno. Worth benchmarking if you're saturating it.
- **`tokio-websockets`** — newer pure-Tokio implementation aiming at a leaner dep tree.
- **`ws-rs`** — older, predates tokio's stabilisation, effectively unmaintained.
- **Browser side** — use `gloo-net::websocket` or raw `web-sys::WebSocket`; not tungstenite (it doesn't run in WASM).

Gotchas:

- **Backpressure isn't automatic.** A slow consumer with a fast producer will buffer messages indefinitely; cap your channels.
- **Close-frame semantics.** RFC-6455 requires a handshake on close; check `Message::Close` and respond, don't just drop the socket.
- **Permessage-deflate.** Off by default; enable via the `deflate` feature if your peers expect it.
- **TLS.** Use `tokio-tungstenite` with the `native-tls`, `rustls-tls-native-roots`, or `rustls-tls-webpki-roots` features; picking the wrong cert source is the #1 wss:// footgun.

## Similar / related topics

- **`tokio-tungstenite`** — the async wrapper; the real default.
- **`fastwebsockets`** — high-throughput alternative from Deno's stack.
- **`tokio-websockets`** — leaner pure-Tokio implementation.
- **`axum::extract::WebSocketUpgrade`** — the framework-level convenience.
- **`gloo-net::websocket`** — browser-side WebSocket client over `web-sys`.

## Internal links
- [[axum]] — its WebSocket extractor wraps `tokio-tungstenite`
- [[http]] — WebSockets ride on top of an HTTP/1.1 upgrade
- [[gloo]] — browser-side WebSocket client for WASM
- [[tokio]] — required runtime for the async variant

## Keywords

`#tungstenite` `#websocket` `#rust` `#tokio` `#async` `#networking`

## References / raw notes

- Crate: <https://crates.io/crates/tungstenite>
- Async wrapper: <https://crates.io/crates/tokio-tungstenite>
- Repo: <https://github.com/snapview/tungstenite-rs>

> Lightweight stream-based WebSocket implementation for Rust.

Sync echo server (small enough to keep as a teaching example):

```rust
use std::net::TcpListener;
use std::thread::spawn;
use tungstenite::accept;

/// A WebSocket echo server.
fn main() {
    let server = TcpListener::bind("127.0.0.1:9001").unwrap();
    for stream in server.incoming() {
        spawn(move || {
            let mut websocket = accept(stream.unwrap()).unwrap();
            loop {
                let msg = websocket.read().unwrap();
                // Don't bounce ping/pong control frames back.
                if msg.is_binary() || msg.is_text() {
                    websocket.send(msg).unwrap();
                }
            }
        });
    }
}
```

For anything async, the same shape using `tokio-tungstenite::accept_async` and a `tokio::spawn` per connection is the standard idiom — see the upstream `examples/` directory.
