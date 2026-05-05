---
title: Salsa
main_link: https://github.com/salsa-rs/salsa?tab=readme-ov-file
keywords: [salsa, concurrency, rust, programming, inputs, key, queries, function]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Salsa

**Main link:** <https://github.com/salsa-rs/salsa?tab=readme-ov-file>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#salsa` `#concurrency` `#rust` `#programming` `#inputs` `#key` `#queries` `#functions`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Salsa

https://github.com/salsa-rs/salsa?tab=readme-ov-file


A generic framework for on-demand, incrementalized computation.

## Key idea
The key idea of salsa is that you define your program as a set of queries. Every query is used like function K -> V that maps from some key of type K to a value of type V. Queries come in two basic varieties:

- Inputs: the base inputs to your system. You can change these whenever you like.
- Functions: pure functions (no side effects) that transform your inputs into other values. The results of queries are memoized to avoid recomputing them a lot. When you make changes to the inputs, we'll figure out (fairly intelligently) when we can re-use these memoized values and when we have to recompute them.


https://salsa-rs.github.io/salsa/overview.html


https://salsa-rs.github.io/salsa/tutorial/ir.html
