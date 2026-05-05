---
title: PUFF
main_link: https://github.com/hansonkd/puff
keywords: [python, interop, rust, programming, level, graphql]
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

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[maturin]] — maturin _(score 33)_
- [[ballista_py]] — ballista py _(score 28)_
- [[pyo3]] — pyo3 _(score 28)_
- [[to_python]] — Rust - Python interactions _(score 23)_
- [[rtic]] — RTIC _(score 20)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#python` `#interop` `#rust` `#programming` `#high` `#level` `#build` `#graphql`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 3 additional top-level heading(s) extracted into sibling files:
> - [pyo3](pyo3.md)
> - [ballista py](ballista_py.md)
> - [maturin](maturin.md)


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
