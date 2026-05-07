---
title: HyperFS
main_link: https://crates.io/crates/hyperfs
keywords: [hyperfs, rust, http-server, static-files, hyper]
status: reviewed
review_date: 2026/05/03
---

# HyperFS

**Main link:** <https://crates.io/crates/hyperfs>

## Summary

`hyperfs` is a tiny single-binary HTTP server that serves the contents of the directory it's run from — Rust's answer to `python3 -m http.server`. Built on `hyper`, ships as one executable, no config. Hobby project; the author admits part of the motivation was to learn Rust.

## Insight

Reach for it (or one of its alternatives) when you need a one-shot static file server for local development, demo handoffs, or sharing a build output over LAN — anywhere starting a Python interpreter would be the alternative. For anything in production, use a real static file server (`nginx`, `caddy`) or `tower-http`'s `ServeDir` mounted in an Axum router.

There are several other Rust crates in the same niche worth being aware of:

- **`miniserve`** — single binary, much more popular, supports auth/upload/QR-code/TLS/templated index. Almost certainly the better default.
- **`simple-http-server`** — sibling project to miniserve, similar feature set.
- **`http`** (Cargo `cargo-server` / `dufs`) — `dufs` adds WebDAV, search, upload UI.
- **`basic-http-server`** — from the rust-lang-nursery; deliberately minimal.

So `hyperfs` is mostly interesting as a small *example* of a hyper-based server you can read end-to-end.

## Similar / related topics

- **`miniserve`** — feature-rich one-binary static server (probably what you actually want).
- **`dufs`** — modern static + WebDAV + upload UI.
- **`python3 -m http.server`** — the cross-tool baseline.
- **`tower-http::ServeDir`** — embed the same behaviour in an Axum app.
- **`caddy`** — production single-binary server with auto-TLS.

## Internal links
- [[http]] — the `hyper` library this is built on
- [[axum]] — for non-trivial servers, mount `tower-http::ServeDir` in Axum instead
- [[tools/shell/README|shell tools]] — sibling small CLIs

## Keywords

`#hyperfs` `#rust` `#http-server` `#static-files` `#cli` `#hyper`

## References / raw notes

- Crate: <https://crates.io/crates/hyperfs>

From the crate description:

> A simple HTTP server for static files. HyperFS is a very simple HTTP server that simply serves the files in the directory it is run from. It is primarily intended as a development tool similar to using `python -m SimpleHTTPServer`. But it is a single lightweight executable written in Rust.
>
> To be honest, part of the motivation was to just so I could try out Rust.

Take this article more as a pointer than an endorsement — `miniserve` is the de-facto Rust replacement for `python -m http.server` in 2025.
