---
title: Symphonia
main_link: https://github.com/pdeljanov/Symphonia
keywords: [symphonia, misc, rust, programming, audio, providing, api, features]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/misc/audio.md` by `article_split.py`. Heading: **Symphonia**.

# Symphonia

**Main link:** <https://github.com/pdeljanov/Symphonia>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[audio]] — Coreaudio _(score 33)_
- [[rtic]] — RTIC _(score 28)_
- [[rodio]] — Rodio _(score 23)_
- [[windows]] — Windows RS _(score 23)_
- [[lewton]] — lewton _(score 23)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#symphonia` `#misc` `#rust` `#programming` `#audio` `#providing` `#api` `#features`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Symphonia

https://github.com/pdeljanov/Symphonia


Symphonia is a pure Rust audio decoding and media demuxing library supporting AAC, ADPCM, ALAC, FLAC, MKV, MP1, MP2, MP3, MP4, OGG, Vorbis, WAV, and WebM.

Features
- Decode support for the most popular audio codecs with support for gapless playback
- Demux the most common media container formats
- Read most metadata and tagging formats
- Automatic format and decoder detection
- Basic audio primitives for manipulating audio data efficiently
- 100% safe Rust
- Minimal dependencies
- Fast with no compromises in performance!


Additionally, planned features include:

- Providing a C API for integration into other languages
- Providing a WASM API for web usage
