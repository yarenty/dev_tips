---
title: Rust audio ecosystem (overview)
main_link: https://github.com/RustAudio
keywords: [audio, rust, ecosystem, codec, playback, cpal, symphonia]
status: reviewed
review_date: 2026/05/03
---

# Rust audio ecosystem (overview)

**Main link:** <https://github.com/RustAudio>

## Summary

Audio in Rust is split across three concerns: **device I/O** (talk to the OS sound system), **codec / container** (decode MP3, WAV, FLAC, OGG, AAC, …), and **playback / mixing** (decoded samples → speakers, with volume, pitch, queuing). The ecosystem is mostly clustered around the [RustAudio](https://github.com/RustAudio) GitHub org. Today's "default" stack for an application that just needs to play sound is **`cpal` (device I/O) + `symphonia` (codec) + `rodio` (high-level playback)** — `rodio` already wires those together for you. The original codec-per-format crates (`claxon`, `hound`, `lewton`, `minimp3`) still exist and are still useful when you need a small, single-format dependency.

## Insight

A short decision tree:

| You want to… | Reach for |
|---|---|
| Just play a sound from a file or buffer | [[rodio]] |
| Decode any common audio file (MP3/FLAC/WAV/OGG/AAC/ALAC/…) | [[symphonia]] |
| Talk to the OS audio device directly (record, low-latency synth) | [`cpal`](https://github.com/RustAudio/cpal) |
| Decode only WAV (read or write) | [[hound]] |
| Decode only FLAC | [[claxon]] |
| Decode only Vorbis (.ogg) | [[lewton]] |
| Decode only MP3 | [[minimp3]] (C-backed) or `puremp3` / `symphonia` |
| Apple Core Audio bindings | `coreaudio-rs` (this article's original main link) |
| Build a synth / DAW / DSP pipeline | [`fundsp`](https://github.com/SamiPerttu/fundsp), [`dasp`](https://github.com/RustAudio/dasp), [`kira`](https://github.com/tesselode/kira) |

A few sharp edges worth knowing:

- **`cpal` is the bottom of the stack.** Almost every higher-level Rust audio crate sits on top of it. When something fails on a particular OS, the bug is often in `cpal` or in the OS-specific backend (CoreAudio, WASAPI, ALSA/PulseAudio).
- **`symphonia` has largely subsumed the per-format crates** for read-only decoding. Pemistahl-style pure-Rust, well-maintained, fuzz-tested. Use it as the default unless your binary size or feature set demands a single-format crate.
- **`coreaudio-rs`** (the file's historical main link) is a thin safe wrapper over Apple's Core Audio C API. You only need it if you're calling Core Audio directly — if you only want to play sound on macOS, `cpal` / `rodio` already handle that for you.
- **MP3 is patent-expired** (since 2017), so `puremp3` and `symphonia`'s MP3 decoder are now legally fine; `minimp3` (C bindings) is still the fastest.

## Similar / related topics

- [`cpal`](https://github.com/RustAudio/cpal) — the cross-platform low-level audio I/O foundation.
- [`kira`](https://github.com/tesselode/kira) — game-oriented audio (tweens, mixers, spatialisation).
- [`fundsp`](https://github.com/SamiPerttu/fundsp) — audio DSP / synthesis graph for music apps.
- [`dasp`](https://github.com/RustAudio/dasp) — fundamental sample / signal types and combinators.
- [`creek`](https://github.com/MeadowlarkDAW/creek) — real-time-safe streaming disk I/O for long files.

## Internal links

- [[symphonia]] — modern unified pure-Rust codec / demuxer.
- [[rodio]] — high-level "play this file" library.
- [[hound]] — WAV-only.
- [[claxon]] — FLAC-only.
- [[lewton]] — Vorbis-only.
- [[minimp3]] — MP3 (Rust bindings to C library).
- [[daktilo]] — fun consumer of audio playback (typewriter sound effects).
- [[README|Misc Rust]] — parent landing page for this kitchen-sink section.

## Keywords

`#audio` `#rust` `#ecosystem` `#cpal` `#rodio` `#symphonia` `#codec` `#playback`

## References / raw notes

- RustAudio org: <https://github.com/RustAudio>
- `coreaudio-rs` (Apple Core Audio safe wrapper): <https://github.com/RustAudio/coreaudio-rs> / <https://crates.io/crates/coreaudio-rs>
- `cpal` (cross-platform I/O foundation): <https://github.com/RustAudio/cpal>
- "Are we audio yet?" landscape: <https://areweaudioyet.com> (community status page; coverage drifts).
