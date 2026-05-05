---
title: Axum
main_link: https://github.com/tokio-rs/axum
keywords: [axum, web, rust, programming, tokio, tower, api, middleware]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/web/http.md` by `article_split.py`. Heading: **Axum**.

# Axum

**Main link:** <https://github.com/tokio-rs/axum>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[http]] — Hyper _(score 33.0)_
- [[feather]] — Feather _(score 29.3)_
- [[rocket]] — Rocket _(score 24.1)_
- [[rtic]] — RTIC _(score 14.7)_
- [[tungstenite]] — Tungstenite _(score 12.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#axum` `#web` `#rust` `#programming` `#tokio` `#tower` `#api` `#middleware`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Axum 

https://github.com/tokio-rs/axum

https://docs.rs/axum/latest/axum/

There is proposition to demote rocket and use Axum instead.
Axum is build by tokio team

https://tokio.rs/blog/2022-11-25-announcing-axum-0-6-0


No macros
No middleware - use tower / tonic / hyper / - easy integration.


Official features:

- Route requests to handlers with a macro-free API.
- Declaratively parse requests using extractors.
- Simple and predictable error handling model.
- Generate responses with minimal boilerplate.
- Take full advantage of the tower and tower-http ecosystem of middleware, services, and utilities.

https://medium.com/@lindblomdev/beginning-rust-by-exploring-a-very-basic-axum-web-api-in-detail-1f4c87e422e0

https://www.youtube.com/watch?v=XZtlD_m59sM  - full course
