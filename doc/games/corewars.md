---
title: Core Wars
main_link: https://github.com/richpl/MARS
keywords: [corewars, games, mov, mars, jmp, rust, python]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Core Wars

**Main link:** <https://github.com/richpl/MARS>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[zxspectrum]] — Rust ZX _(score 24.2)_
- [[game_in_scala]] — GAME in Scala _(score 23.1)_
- [[rtic]] — RTIC _(score 13.1)_
- [[reflection]] — reflect _(score 13.1)_
- [[low_level_structures]] — HalfBrown _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#corewars` `#games` `#mov` `#mars` `#jmp` `#core`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Core Wars



## MARS

https://github.com/richpl/MARS

CoreWars Red Code simulator written in Python, unfinished

Roughly 88 standard, with immediate (#), direct ($) and indirect (@) addressing modes.

Valid instructions:

JMP #A JMP $A

MOV #A, #B MOV $A, #B MOV #A, $B MOV #A, @B MOV $A, $B MOV $A, @B
