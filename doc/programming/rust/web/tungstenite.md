---
title: Tungstenite
main_link: https://crates.io/crates/tungstenite
keywords: [tungstenite, rust, lightweight]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/web/http.md` by `article_split.py`. Heading: **Tungstenite**.

# Tungstenite

**Main link:** <https://crates.io/crates/tungstenite>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rewquest]] — Rewquest _(score 27.5)_
- [[http]] — Hyper _(score 27.5)_
- [[wasmtime]] — wasmtime _(score 27.5)_
- [[html]] — kuchiki _(score 27.5)_
- [[webassembly]] — WASM _(score 24.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tungstenite` `#web` `#rust` `#programming` `#crates` `#lightweight` `#websocket` `#take`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

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
