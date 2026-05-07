---
title: minimp3 — Rust bindings to the C minimp3 decoder
main_link: https://github.com/germangb/minimp3-rs
keywords: [minimp3, rust, mp3, audio, decoder, ffi]
status: reviewed
---

# minimp3 — Rust bindings to the C minimp3 decoder

**Main link:** <https://github.com/germangb/minimp3-rs>

## Summary

`minimp3` is a Rust wrapper (`minimp3` + `minimp3-sys`) around [`lieff/minimp3`](https://github.com/lieff/minimp3), a tiny single-header C MP3 decoder with SSE/NEON SIMD acceleration. It's been the historical Rust default for "decode an MP3" because the underlying C is small, fast, and ISO-conformant. Maintainership of the Rust binding has changed hands (@germangb → @phip1611) and the crate currently carries an explicit "**not recommended for new projects**" warning due to multiple memory-unsoundness issues in the wrapper.

## Insight

For new code, prefer **[[symphonia]]** (pure-Rust MP3 decoder, well-fuzzed, also handles many other formats) or [`nanomp3`](https://crates.io/crates/nanomp3) (small, safe, pure-Rust port of minimp3). Reach for `minimp3-rs` only if you have an existing dependency on it, or you specifically need the SIMD-accelerated C decoder and accept the FFI risk. The C upstream `lieff/minimp3` is still solid; the issue is the Rust safety boundary in the wrapper. Note also that MP3 patents fully expired in 2017, so there is no longer a legal obstacle to shipping pure-Rust MP3 decoders.

For [[rodio]], MP3 decoding now defaults to Symphonia even though the `minimp3` backend remains selectable.

## Similar / related topics

- [[symphonia]] — pure-Rust MP3 decoder; the recommended modern default.
- [`nanomp3`](https://crates.io/crates/nanomp3) — pure-Rust port of minimp3; safe, no FFI.
- [`puremp3`](https://crates.io/crates/puremp3) — older pure-Rust MP3 decoder.
- [`mp3-rs`](https://crates.io/crates/mp3) / [`minimp3-fixed`](https://crates.io/crates/minimp3-fixed) — community forks attempting safety patches.
- [[claxon]] / [[lewton]] / [[hound]] — single-format Rust audio crates for FLAC / Vorbis / WAV.

## Internal links

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[symphonia]] — modern recommended replacement.
- [[rodio]] — uses Symphonia by default for MP3, with minimp3 as a fallback feature.
- [[claxon]] — FLAC sibling.
- [[lewton]] — Vorbis sibling.

## Keywords

`#minimp3` `#rust` `#mp3` `#audio` `#decoder` `#ffi` `#deprecated`

## References / raw notes

- Rust binding repo: <https://github.com/germangb/minimp3-rs>
- Upstream C library: <https://github.com/lieff/minimp3>
- Rust wrapper carries an explicit "not recommended for new projects" warning; suggested replacements are `symphonia` and `nanomp3`.
- Original C library is still actively useful in C/C++ projects; SSE + NEON, ISO-conformant, single-header.
