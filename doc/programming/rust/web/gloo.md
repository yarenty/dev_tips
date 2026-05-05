---
title: Gloo
main_link: 
keywords: [gloo, web, rust, programming, libraries, toolkit, wasm]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/web/webassembly.md` by `article_split.py`. Heading: **Gloo**.

# Gloo

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[webassembly]] — WASM _(score 33)_
- [[rtic]] — RTIC _(score 20)_
- [[rocket]] — Rocket _(score 18)_
- [[yew]] — YEW _(score 18)_
- [[tungstenite]] — Tungstenite _(score 18)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#gloo` `#web` `#rust` `#programming` `#apis` `#libraries` `#toolkit` `#wasm`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Gloo
A toolkit for building fast, reliable Web applications and libraries with Rust and Wasm.


Gloo is a collection of libraries, and those libraries provide ergonomic Rust wrappers for browser APIs. web-sys/js-sys are very difficult/inconvenient to use directly so provides wrappers around the raw bindngs which makes it easier to consume those APIs. This is why it is called a "toolkit", instead of "library" or "framework".
