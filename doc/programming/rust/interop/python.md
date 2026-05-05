---
title: PUFF
main_link: https://github.com/hansonkd/puff
keywords: [python, interop, rust, programming, high, level, build, graphql]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# PUFF

**Main link:** <https://github.com/hansonkd/puff>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#python` `#interop` `#rust` `#programming` `#high` `#level` `#build` `#graphql`

## TODO

- This file contains **4 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# PUFF


https://github.com/hansonkd/puff

It's an experiment to minimize the barrier between Python and Rust to unlock the full potential of high level languages. Build your own Runtime using standard CPython and extend it with Rust. Imagine if GraphQL, Postgres, Redis and PubSub were part of the standard library. That's Puff.

High level overview is that Puff gives Python

- Greenlets on Rust's Tokio.
- High performance HTTP Server - combine Axum with Python WSGI apps (Flask, Django, etc.)
- Rust / Python natively in the same process, no sockets or serialization.
- AsyncIO / uvloop / ASGI integration with Rust
- An easy-to-use GraphQL service
- Multi-node pub-sub
- Rust level Redis Pool
- Rust level Postgres Pool
- Websockets
- semi-compatible with Psycopg2 (hopefully good enough for most of Django)
- A safe convenient way to drop into rust for maximum performance





# pyo3

all 





# ballista py

https://github.com/hyperium/tonic/blob/master/examples/src/helloworld/server.rs



# maturin

https://github.com/PyO3/maturin

Build and publish crates with pyo3, cffi and uniffi bindings as well as rust binaries as python packages with minimal configuration. It supports building wheels for python 3.8+ on Windows, Linux, macOS and FreeBSD, can upload them to pypi and has basic PyPy and GraalPy support.
