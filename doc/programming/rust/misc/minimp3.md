---
title: minimp3
main_link: https://github.com/lieff/minimp3
keywords: [minimp3, rust, minimalistic, mp3]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/misc/audio.md` by `article_split.py`. Heading: **minimp3**.

# minimp3

**Main link:** <https://github.com/lieff/minimp3>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rtic]] — RTIC _(score 17.1)_
- [[hound]] — Hound _(score 17.1)_
- [[programming/rust/misc/drogue|drogue]] — Drogue _(score 17.1)_
- [[daktilo]] — Daktilo _(score 17.1)_
- [[claxon]] — Claxon _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#minimp3` `#misc` `#rust` `#programming` `#minimalistic` `#mp3` `#sse` `#neon`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# minimp3

https://github.com/lieff/minimp3


Minimalistic, single-header library for decoding MP3. minimp3 is designed to be small, fast (with SSE and NEON support), and accurate (ISO conformant). You can find a rough benchmark below, measured using perf on an i7-6700K, IO included, no CPU heat to address speedstep:
