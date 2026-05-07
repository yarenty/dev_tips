---
title: Hound — pure-Rust WAV reader / writer
main_link: https://github.com/ruuda/hound
keywords: [hound, rust, wav, audio, encoder, decoder]
status: reviewed
review_date: 2026/05/03
---

# Hound — pure-Rust WAV reader / writer

**Main link:** <https://github.com/ruuda/hound>

## Summary

Hound is a small, focused, pure-Rust library for reading and writing WAV (the ubiquitous uncompressed PCM container by Microsoft/IBM). Like its sibling [[claxon]], it's written by [Ruud van Asseldonk](https://github.com/ruuda) — Hound was originally created to test Claxon by transcoding FLAC → WAV. Scope is deliberately tiny, and that's why it's the canonical Rust WAV crate: it's stable, well-documented, and you can audit it in an afternoon.

## Insight

Reach for Hound whenever you need to *write* a WAV (recording, transcoding, generating test fixtures, dumping samples for inspection) — that's the gap [[symphonia]] doesn't fill, since Symphonia is read-only. For *reading* WAV, both work; Hound is smaller, Symphonia is more uniform if you also handle other formats. Limitations to be aware of:

- 24-bit and IEEE-float WAV are supported; obscure variants (ADPCM, A-law, μ-law) are not — those are container-level codecs that Hound treats as opaque.
- Memory-mapped / streaming-very-large-files patterns aren't first-class; Hound expects to seek over a `BufReader`.
- For real-time audio in/out (microphone capture, low-latency playback), use `cpal` directly, not Hound — Hound is for the file format, not the device.

Quick "write a sine wave" pattern:

```rust
let spec = hound::WavSpec {
    channels: 1, sample_rate: 44_100, bits_per_sample: 16,
    sample_format: hound::SampleFormat::Int,
};
let mut writer = hound::WavWriter::create("sine.wav", spec)?;
for t in 0..44_100 {
    let s = (t as f32 / 44_100.0 * 2.0 * std::f32::consts::PI * 440.0).sin();
    writer.write_sample((s * i16::MAX as f32) as i16)?;
}
writer.finalize()?;
```

## Similar / related topics

- [[claxon]] — sibling FLAC decoder by the same author; pair for FLAC ↔ WAV transcoding.
- [[symphonia]] — read-side multi-format alternative (does not write).
- [`wav`](https://crates.io/crates/wav) — older, smaller alternative; less full-featured.
- [`creek`](https://github.com/MeadowlarkDAW/creek) — for streaming very large WAV/FLAC from disk in real time.

## Internal links

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[claxon]] — companion FLAC decoder by the same author.
- [[symphonia]] — modern read-only multi-format alternative.
- [[rodio]] — uses Hound under the hood for the WAV format.

## Keywords

`#hound` `#rust` `#wav` `#audio` `#encoder` `#decoder`

## References / raw notes

- Repo: <https://github.com/ruuda/hound>
- crates.io: <https://crates.io/crates/hound>
- Author: Ruud van Asseldonk (also wrote [[claxon]]).
