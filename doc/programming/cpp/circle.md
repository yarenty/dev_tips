---
title: Circle
main_link: https://www.circle-lang.org/
keywords: [circle, cpp, language, compilers, type, rust]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Circle

**Main link:** <https://www.circle-lang.org/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[abi]] — C++ ABI _(score 26.3)_
- [[reflection]] — reflect _(score 20.5)_
- [[rocket]] — Rocket _(score 20.5)_
- [[alagorithms]] — algorithms _(score 13.4)_
- [[rtic]] — RTIC _(score 13.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#circle` `#cpp` `#programming` `#language` `#compiler` `#types` `#make`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Circle

_Note: Suggested as best macro language_


https://www.circle-lang.org/

Circle is a new C++20 compiler. It's written from scratch and designed for easy extension.

New Circle is a major transformation of the Circle compiler, intended as a response to recent successor language announcements. It focuses on a novel fine-grained versioning mechanism that allows the compiler to fix defects and make the language safer and more productive while maintaining 100% compatibility with existing code assets.

New Circle is the richest C++ compiler yet. Try out:

- choice types,
- pattern matching,
- interfaces and impls,
- language type erasure,
- as-expressions for safer conversions,
- a modern declaration syntax with fn and var keywords to make clearer, less ambiguous declarations,
- a simpler syntax for binary expressions, greatly reducing the likelihood of bugs caused by confusing operator precedences,
- a forward keyword to take the complexity and bugginess out of forwarding references,
- safer initializer lists, which address ambiguities when calling std::initializerlist constructors and non-std::initializerlist constructors,
- lifting lambdas to pass overload sets as function arguments,
- nine kinds of template parameters to make templates far more comprehensive,
- reflection traits to access packs of information about class types, enum types, function types, class specializations, and so on,
- pack traits for pack-transforming algorithms, like sort, unique, count, erase, difference, intersection, and so on.

New Circle describes a path for evolving C++ to meet the needs of institutional users. The versioning mechanism that accommodated the development of the features above will also accommodate research into critically important areas like memory safety. Rather than insisting on a one-size-fit's-all approach to language development, project leads can opt into collections of features that best target their projects' needs.
