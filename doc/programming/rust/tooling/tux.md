---
title: tux
main_link: https://crates.io/crates/tux
keywords: [tux, tooling, rust, programming, test, crates, miscellaneous, utilities]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/tooling/tests.md` by `article_split.py`. Heading: **tux**.

# tux

**Main link:** <https://crates.io/crates/tux>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tests]] — mockall _(score 28)_
- [[rtic]] — RTIC _(score 25)_
- [[starship]] — starship _(score 23)_
- [[debug]] — Debug _(score 23)_
- [[programming/rust/tooling/bottom|bottom]] — bottom _(score 23)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#tux` `#tooling` `#rust` `#programming` `#test` `#crates` `#miscellaneous` `#utilities`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# tux

https://crates.io/crates/tux


Miscellaneous test utilities for unit and integration tests in Rust.

[dev-dependencies]
tux = { version = "0.2" }
The goal of this project is to support a variety of test scenarios that may be tricky to test, such as HTTP requests, executing binaries, testing complex output, etc.

There is no particular scope with the code utilities provided, other than being generally useful in a unit or integration test scenario.


HAS!!! temp dirs / files  and HTTP requests
