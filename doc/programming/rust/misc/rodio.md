---
title: Rodio
main_link: https://github.com/RustAudio/rodio
keywords: [rodio, rust, symphonia, playback]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/misc/audio.md` by `article_split.py`. Heading: **Rodio**.

# Rodio

**Main link:** <https://github.com/RustAudio/rodio>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[symphonia]] — Symphonia _(score 25.3)_
- [[rtic]] — RTIC _(score 17.1)_
- [[minimp3]] — minimp3 _(score 17.1)_
- [[lewton]] — lewton _(score 17.1)_
- [[hound]] — Hound _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#rodio` `#misc` `#rust` `#programming` `#symphonia` `#playback` `#handled` `#format`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Rodio

https://crates.io/crates/rodio

Rust playback library.

Playback is handled by cpal. Format decoding can be handled either by Symphonia, or by format-specific decoders:

- MP3 by minimp3 (but defaults to Symphonia).
- WAV by hound.
- Vorbis by lewton.
- FLAC by claxon.
- MP4 and AAC (both disabled by default) are handled only by Symphonia.
See the docs for more details on backends.


https://github.com/RustAudio/rodio
