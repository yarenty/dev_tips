---
title: Claxon — pure-Rust FLAC decoder
main_link: https://github.com/ruuda/claxon
keywords: [claxon, rust, flac, decoder, audio]
status: reviewed
---

# Claxon — pure-Rust FLAC decoder

**Main link:** <https://github.com/ruuda/claxon>

## Summary

Claxon is a pure-Rust FLAC (Free Lossless Audio Codec) decoder by [Ruud van Asseldonk](https://github.com/ruuda). It has been fuzzed and verified against the reference FLAC decoder for correctness, and its decode performance is roughly on par with the reference C implementation. Scope is deliberately narrow: read FLAC files (and the metadata blocks inside them), no encoding, no other formats.

## Insight

Reach for Claxon when you need a small, single-purpose, well-audited FLAC decoder and don't want to pull in the full `symphonia` codec graph. Pair it with [[hound]] (also by ruuda) when transcoding FLAC → WAV. For modern multi-format apps the recommended default is now [[symphonia]], which decodes FLAC alongside many other formats; Claxon is still the lighter, narrower dependency. Note: encoding FLAC from Rust still effectively means shelling out to the reference `flac` CLI or wrapping `libFLAC` — there is no production-grade pure-Rust FLAC *encoder*.

Minimal RMS-of-a-FLAC-file example:

```rust
let mut reader = claxon::FlacReader::open("testsamples/pop.flac").unwrap();
let mut sqr_sum = 0.0;
let mut count = 0;
for sample in reader.samples() {
    let s = sample.unwrap() as f64;
    sqr_sum += s * s;
    count += 1;
}
println!("RMS is {}", (sqr_sum / count as f64).sqrt());
```

See `examples/decode_simple.rs` (Claxon + Hound → WAV) and `examples/decode.rs` (faster, format-aware) in the repo.

## Similar / related topics

- [[symphonia]] — modern unified codec library; decodes FLAC and much more.
- [[hound]] — WAV reader/writer by the same author; natural Claxon companion for transcoding.
- [`libFLAC`](https://xiph.org/flac/) — the C reference encoder/decoder if you need encoding.
- [`metaflac`](https://crates.io/crates/metaflac) — FLAC metadata-only reader if you don't need samples.

## Internal links

<!-- reviewed -->

- [[audio]] — Rust audio ecosystem overview / decision tree.
- [[symphonia]] — modern unified replacement.
- [[hound]] — WAV companion crate by the same author.
- [[lewton]] — Vorbis sibling (the .ogg companion to Claxon's .flac).
- [[minimp3]] — MP3 sibling.

## Keywords

`#claxon` `#rust` `#flac` `#decoder` `#audio` `#pure-rust`

## References / raw notes

- Repo: <https://github.com/ruuda/claxon>
- Author: Ruud van Asseldonk (also wrote [[hound]]).
- Relationship: Hound was originally written to *test* Claxon.
- Examples: `examples/decode_simple.rs`, `examples/decode.rs` in the repo.
