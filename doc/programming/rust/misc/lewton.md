---
title: lewton — pure-Rust Vorbis decoder
main_link: https://github.com/RustAudio/lewton
keywords: [lewton, rust, vorbis, ogg, audio, decoder]
status: reviewed
---

# lewton — pure-Rust Vorbis decoder

**Main link:** <https://github.com/RustAudio/lewton>

## Summary

`lewton` is a pure-Rust decoder for the Xiph Vorbis audio codec (the lossy codec inside `.ogg` files). It lives under the [RustAudio](https://github.com/RustAudio) umbrella and is the Vorbis sibling of [[claxon]] (FLAC) and [[minimp3]] (MP3). It handles the standard Vorbis-in-Ogg packaging, exposes packets as `i16` PCM frames, and ships a runnable `examples/player.rs` that plays a `.ogg` file end-to-end.

## Insight

Reach for `lewton` when you need a small, focused Vorbis decoder and don't want the breadth (or compile time) of [[symphonia]]. It's the Vorbis backend that [[rodio]] uses by default. Decode-only — there is no encoder; encoding Vorbis from Rust still means wrapping `libvorbis` C bindings or using `ffmpeg`. Vorbis itself is patent-free and royalty-free, so this crate is safe to ship in any product without licensing worry.

Quick smoke test from the repo:

```sh
cargo run --example player /path/to/your/audio_file.ogg
```

## Similar / related topics

- [[symphonia]] — modern unified codec library; Vorbis is one of many it decodes.
- [[claxon]] — sibling FLAC decoder.
- [[minimp3]] — sibling MP3 decoder.
- [`opus`](https://crates.io/crates/opus) / [`audiopus`](https://crates.io/crates/audiopus) — Vorbis's modern successor (used by Discord, WebRTC, etc.).
- [`ogg`](https://crates.io/crates/ogg) — the Ogg-container layer that lewton sits on top of.

## Internal links

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[symphonia]] — modern unified replacement.
- [[rodio]] — uses lewton as its default Vorbis backend.
- [[claxon]] — FLAC sibling.
- [[minimp3]] — MP3 sibling.

## Keywords

`#lewton` `#rust` `#vorbis` `#ogg` `#audio` `#decoder` `#rustaudio`

## References / raw notes

- Repo: <https://github.com/RustAudio/lewton>
- crates.io: <https://crates.io/crates/lewton>
- Examples: `examples/player.rs` plays a `.ogg` file using cpal under the hood.
