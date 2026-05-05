---
title: turmoil
main_link: https://crates.io/crates/turmoil
keywords: [turmoil, rust, tokio, distributed]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/tooling/tests.md` by `article_split.py`. Heading: **turmoil**.

# turmoil

**Main link:** <https://crates.io/crates/turmoil>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tests]] — mockall _(score 17.1)_
- [[starship]] — starship _(score 17.1)_
- [[bandwhich]] — Bandwhich _(score 17.1)_
- [[debug]] — Debug _(score 17.1)_
- [[tracing]] — Tracing _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#turmoil` `#tooling` `#rust` `#programming` `#crates` `#tokio` `#network` `#written`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# turmoil

https://crates.io/crates/turmoil

written by tokio team
https://tokio.rs/blog/2023-01-03-announcing-turmoil


Turmoil is a framework for testing distributed systems. It provides deterministic execution by running multiple concurrent hosts within a single thread. It introduces "hardship" into the system via changes in the simulated network. The network can be controlled manually or with a seeded rng.
