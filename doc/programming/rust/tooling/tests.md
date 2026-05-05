---
title: mockall
main_link: https://crates.io/crates/mockall
keywords: [rust, mock]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# mockall

**Main link:** <https://crates.io/crates/mockall>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[turmoil]] — turmoil _(score 35.3)_
- [[starship]] — starship _(score 26.4)_
- [[debug]] — Debug _(score 26.4)_
- [[tux]] — tux _(score 22.1)_
- [[rtic]] — RTIC _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tests` `#tooling` `#rust` `#programming` `#crates` `#test` `#mock` `#turmoil`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 2 additional top-level heading(s) extracted into sibling files:
> - [tux](tux.md)
> - [turmoil](turmoil.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# mockall

https://crates.io/crates/mockall

Mock objects are a powerful technique for unit testing software. A mock object is an object with the same interface as a real object, but whose responses are all manually controlled by test code. They can be used to test the upper and middle layers of an application without instantiating the lower ones, or to inject edge and error cases that would be difficult or impossible to create when using the full stack.

Statically typed languages are inherently more difficult to mock than dynamically typed languages. Since Rust is a statically typed language, previous attempts at creating a mock object library have had mixed results. Mockall incorporates the best elements of previous designs, resulting in it having a rich feature set with a terse and ergonomic interface. Mockall is written in 100% safe and stable Rust.
