---
title: Reflection step by step - Article
main_link: https://www.osohq.com/post/rust-reflection-pt-1
keywords: [reflection-step-by-step-article, core, rust, programming, reflection, articles, runtimes]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/core/reflection.md` by `article_split.py`. Heading: **Reflection step by step - Article**.

# Reflection step by step - Article

**Main link:** <https://www.osohq.com/post/rust-reflection-pt-1>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[reflection]] — reflect _(score 33.5)_
- [[error]] — eyre _(score 24.7)_
- [[low_level_structures]] — HalfBrown _(score 24.7)_
- [[pin_project]] — pin-project _(score 24.7)_
- [[articles]] — Articles _(score 20.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#reflection-step-by-step-article` `#core` `#rust` `#programming` `#reflection` `#step` `#article` `#runtime`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Reflection step by step - Article

using Any!

https://www.osohq.com/post/rust-reflection-pt-1

he foundation of our runtime reflection system through classes and instances, and we've shown some simple dynamic type checking using the built in Any trait.

Up next, things start getting a bit more complicated as we attempt to replicate Python's getattr magic method, and make it possible to look up attributes on Rust structs dynamically at runtime.
