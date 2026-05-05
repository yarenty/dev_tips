---
title: HalfBrown
main_link: https://crates.io/crates/halfbrown
keywords: [low-level-structures, core, rust, programming, crates, tinyvec, hashmap, vec]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# HalfBrown

**Main link:** <https://crates.io/crates/halfbrown>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#low-level-structures` `#core` `#rust` `#programming` `#crates` `#tinyvec` `#hashmap` `#vec`

## TODO

- This file contains **4 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# HalfBrown

https://crates.io/crates/halfbrown


Hashmap implementation that dynamically switches from a vector based backend to a hashbrown based backend as the number of keys grows

Note: The heavy lifting in this is done in hashbrown, and the docs and API are copied from them.

Halfbrown, is a hashmap implementation that uses two backends to optimize for different cernairos:


# hashbrown
 now part of official rust hashmap


# pin-project

A crate for safe and ergonomic pin-projection.

https://crates.io/crates/pin-project



# tinyvec

https://crates.io/crates/tinyvec


A 100% safe crate of vec-like types. #![forbid(unsafe_code)]

Main types are as follows:

ArrayVec is an array-backed vec-like data structure. It panics on overflow.
SliceVec is the same deal, but using a &mut [T].
TinyVec (alloc feature) is an enum that's either an Inline(ArrayVec) or a Heap(Vec). If a TinyVec is Inline and would overflow it automatically transitions to Heap and continues whatever it was doing.
To attain this "100% safe code" status there is one compromise: the element type of the vecs must implement Default.
