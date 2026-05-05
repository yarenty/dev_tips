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

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tinyvec]] — tinyvec _(score 42.4)_
- [[hashbrown]] — hashbrown _(score 33.5)_
- [[error]] — eyre _(score 28.0)_
- [[pin_project]] — pin-project _(score 28.0)_
- [[eyre]] — eyre _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#low-level-structures` `#core` `#rust` `#programming` `#crates` `#tinyvec` `#hashmap` `#vec`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 3 additional top-level heading(s) extracted into sibling files:
> - [hashbrown](hashbrown.md)
> - [pin-project](pin_project.md)
> - [tinyvec](tinyvec.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# HalfBrown

https://crates.io/crates/halfbrown


Hashmap implementation that dynamically switches from a vector based backend to a hashbrown based backend as the number of keys grows

Note: The heavy lifting in this is done in hashbrown, and the docs and API are copied from them.

Halfbrown, is a hashmap implementation that uses two backends to optimize for different cernairos:
