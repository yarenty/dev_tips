---
title: 7 more Rust project ideas
main_link: https://medium.com/
keywords: [project-ideas, rust, learning, web-scraper, blockchain, chat, game-engine]
status: reviewed
---

# 7 more Rust project ideas

**Main link:** <https://medium.com/>

## Summary

A short brainstorm list of seven non-trivial Rust project ideas to practise on, each chosen to exercise a specific corner of the ecosystem: web scraping (Actix + reqwest), file encryption (CLI + crypto crates), parallel image processing (rayon/threads + `image`), a toy blockchain, a Rocket-based blog, a WebSocket chat server, and a simulation game in a Rust game engine. Sister list to [[from_easy_to_advanced]] (15 progressively harder ZTM projects); these seven are flatter and more "pick the one that excites you".

## Insight

Project-driven learning beats theory once you've read the basics; the trick is picking projects that exercise *one new thing* without you also re-learning a problem domain. The seven here are scoped that way — for example "WebSocket chat" exercises async + shared state + framing, but the chat protocol is trivial. Some specifics deserve modern updates: **Amethyst is archived** (use [Bevy](https://bevyengine.org/) instead — see this vault's `games/engines.md`); **`ws-rs` is unmaintained** (use `tokio-tungstenite` or `axum`'s WebSocket support); **Actix Web** is fine but `axum` is now the more common pick for new code; the `crypto` crate name has moved around — use the [RustCrypto](https://github.com/RustCrypto) project's audited primitives. The blockchain idea pairs well with [[_todo_ideas]] (port a Python tutorial to Rust). For a structured ladder of difficulty rather than a flat list, see [[from_easy_to_advanced]].

## Similar / related topics

- [[from_easy_to_advanced]] — ZTM's 15-project ladder, structured beginner→advanced.
- [[_todo_ideas]] — single-project idea (Python blockchain → Rust port).
- [Are We Game Yet?](https://arewegameyet.rs/) — current state of Rust game-dev.
- [Awesome Rust](https://github.com/rust-unofficial/awesome-rust) — discover crates per problem area.
- [crates.io categories](https://crates.io/categories) — find the right crate for each project.

## Internal links
<!-- reviewed -->
- [[from_easy_to_advanced]]
- [[_todo_ideas]]
- [[scope]]
- [[README]]

## Keywords

`#project-ideas` `#rust` `#learning` `#practice`

## References / raw notes

The seven ideas (each ~1 paragraph in the original):

1. **Web scraper** — Actix-Web + reqwest. Practice: async HTTP, error handling, clean public API. *Modern note: `axum` or plain `tokio` + `reqwest` is more idiomatic now.*
2. **CLI file encryption** — uses Rust crypto crates. Practice: key management, file I/O, secure defaults. *Modern note: pull primitives from [RustCrypto](https://github.com/RustCrypto) (`aes-gcm`, `chacha20poly1305`).*
3. **Parallel image processing** — the `image` crate + `rayon`/threads. Practice: data parallelism, filter algorithms.
4. **Blockchain implementation** — hashing, PoW, P2P with tokio. Practice: distributed state, networking. See [[_todo_ideas]] for the linked Python tutorial to port.
5. **Personal blog** — Rocket framework. Practice: routing, auth, CRUD, Markdown rendering. *Alt: `axum` + `askama` templates.*
6. **WebSocket chat** — was suggested with `ws-rs` (now stale); use `tokio-tungstenite` or `axum`'s WebSocket extractor. Practice: async, shared state, framing.
7. **Simulation game** — was suggested with [Amethyst](https://amethyst.rs/) (now archived); use [Bevy](https://bevyengine.org/) — ECS-based, actively developed.
