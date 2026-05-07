---
title: Rodio — high-level audio playback for Rust
main_link: https://github.com/RustAudio/rodio
keywords: [rodio, rust, audio, playback, cpal, sound]
status: reviewed
review_date: 2026/05/03
---

# Rodio — high-level audio playback for Rust

**Main link:** <https://github.com/RustAudio/rodio>

## Summary

Rodio is the canonical "play a sound from a file or buffer" library for Rust. Device I/O is delegated to [`cpal`](https://github.com/RustAudio/cpal); decoding is dispatched to [[symphonia]] by default, with the per-format crates ([[hound]] WAV, [[claxon]] FLAC, [[lewton]] Vorbis, [[minimp3]] MP3) selectable as alternative backends via Cargo features. It exposes a `Sink` abstraction with volume, pause, queueing, and basic effects (speed, fade, low-pass, mix), making it the obvious choice for games, CLI tools, and applications that don't need a full DAW-level mixer.

## Insight

Reach for Rodio whenever the requirement is essentially "I have a sound file, play it through the user's speakers." It hides every cross-platform audio-host quirk behind a small idiomatic API. Things to watch out for:

- **It owns the audio thread.** A `Sink` keeps a background thread/stream alive; drop the `OutputStream` and audio stops globally. Hold the `OutputStream` (and `OutputStreamHandle`) for as long as you want sound.
- **Decoding format support depends on enabled features.** Out of the box you get the common formats; for AAC/ALAC/MP4 you may need to enable the corresponding `symphonia-*` features.
- **Not real-time / not low-latency.** For game-shot effects with consistent ms-level latency, [`kira`](https://github.com/tesselode/kira) is a better fit; for synthesizers/DAWs go straight to `cpal` + `fundsp`/`dasp`.
- **Pitch-shifting is naive.** `Source::speed()` just resamples — it changes pitch *and* duration. For pitch-preserving time-stretch use `rubato` or `signalsmith-stretch` bindings.

A minimal play-a-file example:

```rust
use std::fs::File; use std::io::BufReader; use rodio::{Decoder, OutputStream, Sink};

let (_stream, stream_handle) = OutputStream::try_default()?;
let sink = Sink::try_new(&stream_handle)?;
let file = BufReader::new(File::open("ding.mp3")?);
sink.append(Decoder::new(file)?);
sink.sleep_until_end();
```

## Similar / related topics

- [`cpal`](https://github.com/RustAudio/cpal) — the cross-platform device-I/O layer Rodio sits on top of.
- [`kira`](https://github.com/tesselode/kira) — game-oriented playback with tweens, mixers, spatialisation.
- [[symphonia]] — Rodio's default decoding backend.
- [`creek`](https://github.com/MeadowlarkDAW/creek) — real-time-safe streaming for very long files.
- [`fundsp`](https://github.com/SamiPerttu/fundsp) — when you need synthesis / DSP graphs instead of file playback.

## Internal links

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[symphonia]] — default decoder backend.
- [[hound]] — WAV backend.
- [[claxon]] — FLAC backend.
- [[lewton]] — Vorbis backend.
- [[minimp3]] — MP3 backend (Symphonia is now the default).
- [[daktilo]] — example of a Rust app that plays sound via cpal-stack.

## Keywords

`#rodio` `#rust` `#audio` `#playback` `#cpal` `#sound` `#sink`

## References / raw notes

- Repo: <https://github.com/RustAudio/rodio>
- crates.io: <https://crates.io/crates/rodio>
- Stack: device I/O via `cpal`; decoding dispatched to Symphonia by default, with per-format backends:
  - MP3 → minimp3 (defaults to Symphonia)
  - WAV → hound
  - Vorbis → lewton
  - FLAC → claxon
  - MP4 / AAC → Symphonia only (off by default)
- See the docs for backend feature-flag details.
