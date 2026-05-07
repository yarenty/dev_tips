---
title: Symphonia — pure-Rust media demuxer + codec library
main_link: https://github.com/pdeljanov/Symphonia
keywords: [symphonia, rust, audio, codec, demuxer, flac, mp3, aac, vorbis]
status: reviewed
---

# Symphonia — pure-Rust media demuxer + codec library

**Main link:** <https://github.com/pdeljanov/Symphonia>

## Summary

Symphonia (by [Philip Deljanov](https://github.com/pdeljanov)) is a 100% safe, pure-Rust audio decoding and media demuxing library. It supports an unusually broad codec set — AAC, ADPCM, ALAC, FLAC, MKV, MP1/MP2/MP3, MP4, OGG/Vorbis, WAV, WebM — behind a single uniform API, with automatic format and codec detection from a `Read` source. It is the closest thing the Rust audio ecosystem has to a "ffmpeg-decoder-lite": one dependency, most things just work.

## Insight

Symphonia is the **modern unified replacement** for the per-format crates ([[claxon]] FLAC / [[hound]] WAV-decoding / [[lewton]] Vorbis / [[minimp3]] MP3). Reach for it whenever you need to decode "any audio file the user gave us" — media players, transcoders, ML feature extractors, audio analysis, podcast tooling. It is read-only: there is no encoding side, so for *writing* audio you still pair with [[hound]] (WAV) or shell out to ffmpeg. Other things to know:

- **Gapless playback** is a first-class feature (correctly handles the encoder-delay/padding samples on MP3/AAC/Vorbis).
- **Metadata + tagging** (ID3v1/v2, Vorbis comments, MP4/iTunes atoms, FLAC tags) is exposed alongside samples — handy for music players.
- **No `unsafe`**, no C dependencies, no `bindgen` — easy to vendor, easy to cross-compile, easy to audit.
- The default features enable royalty-free codecs only; commercially-encumbered codecs (MP3 used to be one before 2017) are gated behind explicit feature flags out of historical caution.
- Performance is generally within ~10–30% of native C decoders; for batch transcoding workloads where the last 30% matters, ffmpeg via [`ffmpeg-next`](https://crates.io/crates/ffmpeg-next) is still faster.

A planned C API and WASM API are on the roadmap (per the project's stated goals) for non-Rust embedding.

## Similar / related topics

- [[rodio]] — playback library; uses Symphonia as its default decoder backend.
- [`ffmpeg-next`](https://crates.io/crates/ffmpeg-next) — Rust bindings to libavcodec/libavformat for when you also need video / encoding.
- [`creek`](https://github.com/MeadowlarkDAW/creek) — real-time-safe streaming reader on top of Symphonia.
- [[claxon]] / [[hound]] / [[lewton]] / [[minimp3]] — the per-format predecessors Symphonia replaces.
- [`gstreamer-rs`](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs) — alternative when you need a full media pipeline (also handles video).

## Internal links

<!-- reviewed -->

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[rodio]] — playback layer that wires Symphonia + cpal.
- [[claxon]] / [[hound]] / [[lewton]] / [[minimp3]] — the narrower single-format crates Symphonia subsumes.

## Keywords

`#symphonia` `#rust` `#audio` `#codec` `#demuxer` `#pure-rust` `#flac` `#mp3` `#aac` `#vorbis`

## References / raw notes

- Repo: <https://github.com/pdeljanov/Symphonia>
- Author: Philip Deljanov.
- Supported: AAC, ADPCM, ALAC, FLAC, MKV, MP1/MP2/MP3, MP4, OGG/Vorbis, WAV, WebM.
- Highlights: gapless playback, metadata/tag reading, automatic format+codec detection, 100% safe Rust, minimal deps.
- Planned: C API, WASM API.
