---
title: bytemuck
main_link: https://crates.io/crates/bytemuck
keywords: [bytes-manipulation, rust, bits]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# bytemuck

**Main link:** <https://crates.io/crates/bytemuck>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[starship]] — starship _(score 18.0)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 18.0)_
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 18.0)_
- [[programming/rust/tooling/bottom|bottom]] — bottom _(score 18.0)_
- [[programming/rust/bottom|bottom]] — bottom _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#bytes-manipulation` `#rust` `#programming` `#crates` `#bits` `#crate` `#between`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# bytemuck

A crate for mucking around with piles of bytes.

This crate lets you safely perform "bit cast" operations between data types. That's where you take a value and just reinterpret the bits as being some other type of value, without changing the bits.

This is not like the as keyword
This is not like the From trait
It is most like f32::to_bits, just generalized to let you convert between all sorts of data types.


https://crates.io/crates/bytemuck

## bitfrob

https://crates.io/crates/bitfrob
