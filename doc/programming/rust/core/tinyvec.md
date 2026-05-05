---
title: tinyvec
main_link: https://crates.io/crates/tinyvec
keywords: [tinyvec, core, rust, programming, vec, arrayvec, inline, heap]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/core/low_level_structures.md` by `article_split.py`. Heading: **tinyvec**.

# tinyvec

**Main link:** <https://crates.io/crates/tinyvec>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[low_level_structures]] — HalfBrown _(score 42.4)_
- [[reflection_step_by_step_article]] — Reflection step by step - Article _(score 24.7)_
- [[reflection]] — reflect _(score 24.7)_
- [[error]] — eyre _(score 24.7)_
- [[eyre]] — eyre _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tinyvec` `#core` `#rust` `#programming` `#vec` `#arrayvec` `#inline` `#heap`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# tinyvec

https://crates.io/crates/tinyvec


A 100% safe crate of vec-like types. #![forbid(unsafe_code)]

Main types are as follows:

ArrayVec is an array-backed vec-like data structure. It panics on overflow.
SliceVec is the same deal, but using a &mut [T].
TinyVec (alloc feature) is an enum that's either an Inline(ArrayVec) or a Heap(Vec). If a TinyVec is Inline and would overflow it automatically transitions to Heap and continues whatever it was doing.
To attain this "100% safe code" status there is one compromise: the element type of the vecs must implement Default.
