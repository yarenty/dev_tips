---
title: PUFF (CPython embedded in Rust)
main_link: https://github.com/hansonkd/puff
keywords: [puff, python, rust, embedding, runtime, axum, tokio]
status: reviewed
---

# PUFF (CPython embedded in Rust)

**Main link:** <https://github.com/hansonkd/puff>

## Summary

[Puff](https://github.com/hansonkd/puff) is an experimental Rust runtime
that embeds CPython in-process and exposes a batteries-included high-level
API to it — HTTP (Axum), GraphQL, Postgres pools, Redis pools, pub/sub,
and WebSockets — so a Python program can lean on Rust performance without
explicit FFI per call. Architecturally it inverts the [[pyo3|PyO3]]
recipe: instead of *exporting* a small Rust library to Python, it
*hosts* Python inside a Rust runtime and provides Greenlets that are
backed by Tokio tasks. Think "WSGI/ASGI server + standard library
upgrades, written in Rust, exposed back to Python".

## Insight

Puff is the most ambitious of the Rust-hosts-Python projects, but it is a
single-author research-grade experiment — last meaningful activity is
years old at the time of writing, no PyPI release, no production users I'm
aware of. File it under "interesting prior art", not "production choice".

If you want this *kind* of architecture today, your real options are:

- **[[pyo3|PyO3]] embedding mode** — the same import-the-interpreter API,
  without Puff's batteries. You wire your own Tokio bridge.
- **[Robyn](https://github.com/sparckles/robyn)** — actively maintained
  PyO3-based async web framework that exposes Python handlers to a Rust
  Hyper server. Closest to Puff in spirit and shape.
- **[Granian](https://github.com/emmett-framework/granian)** — Rust ASGI
  server for existing Django/Starlette/FastAPI apps. Less ambitious than
  Puff but works today.
- **[uvloop](https://github.com/MagicStack/uvloop)** — when all you wanted
  was "asyncio but faster".

For the inverse direction (write a Rust crate, expose to Python) see
[[pyo3|PyO3]] + [[maturin]] + [[to_python]].

This page also serves as the historical "Python interop overview" parent
that the auto-splitter pulled siblings out of — the canonical
sub-articles are now [[pyo3|PyO3]], [[maturin]], [[ballista_py]], and
[[to_python]].

## Similar / related topics

- [[pyo3|PyO3]] — embedding the interpreter without the framework layer.
- Robyn — actively maintained PyO3-based async web framework.
- Granian — Rust ASGI server for existing Python frameworks.
- uvloop — drop-in fast asyncio loop (libuv-based, not Rust).
- [[ballista_py]] — Apache Ballista's Python bindings (the
  external-process inverse pattern).

## Internal links

- [[pyo3|PyO3]] — canonical Rust↔CPython binding.
- [[maturin]] — build tool for the inverse direction.
- [[to_python]] — Rust→PyPI recipe.
- [[ballista_py]] — Python bindings for distributed DataFusion.
- [[README]] — Rust interop landing.

## Keywords

`#puff` `#python` `#rust` `#embedding` `#runtime` `#axum` `#tokio`

## References / raw notes

- [GitHub — hansonkd/puff](https://github.com/hansonkd/puff)

Original blurb (Kyle Hanson):

> It's an experiment to minimize the barrier between Python and Rust to
> unlock the full potential of high level languages. Build your own
> Runtime using standard CPython and extend it with Rust. Imagine if
> GraphQL, Postgres, Redis and PubSub were part of the standard library.
> That's Puff.

What Puff promises Python:

- Greenlets on Rust's Tokio.
- High performance HTTP server — combine Axum with Python WSGI apps
  (Flask, Django, etc.).
- Rust / Python natively in the same process — no sockets, no
  serialization.
- AsyncIO / uvloop / ASGI integration with Rust.
- An easy-to-use GraphQL service.
- Multi-node pub/sub.
- Rust-level Redis pool.
- Rust-level Postgres pool.
- WebSockets.
- Semi-compatible with Psycopg2 (hopefully good enough for most of
  Django).
- A safe, convenient way to drop into Rust for maximum performance.
