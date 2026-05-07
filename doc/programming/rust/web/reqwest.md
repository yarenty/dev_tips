---
title: reqwest
main_link: https://crates.io/crates/reqwest
keywords: [reqwest, rust, http-client, hyper, async, json]
status: reviewed
---

# reqwest

**Main link:** <https://crates.io/crates/reqwest>

## Summary

`reqwest` is the de-facto async HTTP client for Rust — an ergonomic, batteries-included wrapper over `hyper` (with an optional sync API for simple scripts). Maintained by Sean McArthur, the same author as `hyper`. It bundles JSON / form / multipart bodies, cookies, redirect policies, system or `rustls` TLS, automatic decompression, proxies, connection pooling, and a `wasm32-unknown-unknown` target that proxies through the browser's `fetch` API.

## Insight

Pick `reqwest` for almost any application-level HTTP-client need: it's what every Rust tutorial reaches for, every framework's test suite uses, and every SDK author wraps. The "batteries-included" reputation is real — `Client::new().get(url).send().await?.json::<T>().await?` covers the 80% case in two lines.

When *not* to use it:

- **Hard binary-size budget** (CLI tools, embedded) — pull in `ureq` (sync, blocking, ~10× smaller) or `attohttpc`. `reqwest` drags in the full `hyper`+`tokio`+TLS stack.
- **Single-binary-no-tokio** — same answer, `ureq`.
- **You need to drop below the abstraction** (custom connection pools, raw HTTP/2 frames) — go straight to `[[http|hyper]]`.
- **Multipart streams of huge files** — works but check that you're not buffering; use `Body::wrap_stream`.
- **WASM in browsers** — works but the surface area is restricted (no custom TLS, no cookies you control, no streaming uploads pre-recently); the `gloo-net` and raw `web-sys` `fetch` are sometimes nicer.

Gotchas:

- **Default `rustls` vs native TLS feature flags.** Picking `rustls-tls` removes a system-OpenSSL dep but breaks if a target needs a custom CA bundle.
- **One `Client` per process.** Don't construct a new `Client` per request — you lose connection pooling.
- **`.json()` deserialises the *whole* body before returning.** For huge JSON, stream and use `serde_json::from_reader` on the streamed bytes.
- **Cookies are off by default.** Enable with `.cookie_store(true)`.

## Similar / related topics

- **`ureq`** — sync, no-tokio, small dependency footprint; ideal for CLIs.
- **`isahc`** — async client built on `libcurl` (different feature set, system-curl integration).
- **`hyper-util` `Client`** — the lower-level client `reqwest` wraps; use directly if you need full control.
- **`surf`** — async-std-flavoured client; effectively unmaintained.
- **`gloo-net`** — friendlier `fetch` wrapper for browser-side WASM; see `[[gloo]]`.

## Internal links
- [[http]] — the underlying `hyper` HTTP layer
- [[axum]] — natural server pairing
- [[gloo]] — browser-side `fetch` wrapper for WASM targets
- [[json]] — `serde_json` is the typical body codec

## Keywords

`#reqwest` `#http-client` `#rust` `#hyper` `#async` `#json` `#tls`

## References / raw notes

- Crate: <https://crates.io/crates/reqwest>
- Docs: <https://docs.rs/reqwest>
- Repo: <https://github.com/seanmonstar/reqwest>

From the crate description:

> An ergonomic, batteries-included HTTP client for Rust.
>
> - Plain bodies, JSON, urlencoded, multipart
> - Customizable redirect policy
> - HTTP proxies
> - HTTPS via system-native TLS (or optionally, `rustls`)
> - Cookie store
> - WASM

Note: this article was previously misnamed `rewquest.md` and was renamed during the P5.X review pass.
