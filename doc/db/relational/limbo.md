---
title: Limbo
main_link: https://github.com/tursodatabase/limbo
keywords: [limbo, relational, db, rust, linux, improved]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Limbo

**Main link:** <https://github.com/tursodatabase/limbo>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[singlestore]] — Singlestore _(score 18)_
- [[tensorbase]] — TensorBase _(score 18)_
- [[rtic]] — RTIC _(score 15)_
- [[sqlite]] — sqlite _(score 15)_
- [[indexing]] — Indexing _(score 15)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#limbo` `#relational` `#db` `#rust` `#linux` `#support` `#improved`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Limbo

https://github.com/tursodatabase/limbo

Limbo is a work-in-progress, in-process OLTP database engine library written in Rust that has:

- SQLite compatibility [doc] for SQL dialect, file formats, and the C API
- Language bindings for JavaScript/WebAssembly, Rust, Go, Python, and Java
- Asynchronous I/O support on Linux with io_uring
- OS support for Linux, macOS, and Windows 

In the future, we will be also working on:

- BEGIN CONCURRENT for improved write throughput.
- Indexing for vector search.
- Improved schema management including better ALTER support and strict column types by default.
