---
title: Cacao
main_link: 
keywords: [cacao, gui, rust, programming, appkit, quality, uikit, swift]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/gui/macos.md` by `article_split.py`. Heading: **Cacao**.

# Cacao

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rtic]] — RTIC _(score 20)_
- [[egui]] — egui _(score 18)_
- [[matrix]] — Matrix - rust bindings to ios version _(score 18)_
- [[mobile]] — Mobile _(score 18)_
- [[swww]] — swww _(score 18)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#cacao` `#gui` `#rust` `#programming` `#appkit` `#quality` `#uikit` `#swift`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Cacao

This library provides safe Rust bindings for AppKit on macOS (beta quality, fairly usable) and UIKit on iOS/tvOS (alpha quality, see repo). It tries to do so in a way that, if you've done programming for the framework before (in Swift or Objective-C), will feel familiar. This is tricky in Rust due to the ownership model, but some creative coding and assumptions can get us pretty far.
