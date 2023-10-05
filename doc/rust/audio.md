# Coreaudio

https://crates.io/crates/coreaudio-rs


A friendly rust interface for Apple's Core Audio API.

This crate aims to expose and wrap the functionality of the original C API in a zero-cost, safe, Rust-esque manner.

If you just want direct access to the unsafe bindings, use coreaudio-sys.


https://github.com/RustAudio/coreaudio-rs


# Daktilo

https://github.com/orhun/daktilo

Typewriter sound effect




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




# minimp3

https://github.com/lieff/minimp3


Minimalistic, single-header library for decoding MP3. minimp3 is designed to be small, fast (with SSE and NEON support), and accurate (ISO conformant). You can find a rough benchmark below, measured using perf on an i7-6700K, IO included, no CPU heat to address speedstep:


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



# Hound

https://github.com/ruuda/hound


A wav encoding and decoding library in Rust.

Crates.io version Changelog Documentation

Hound can read and write the WAVE audio format, an ubiquitous format for raw, uncompressed audio. The main motivation to write it was to test Claxon, a FLAC decoding library written in Rust.


# lewton

https://github.com/RustAudio/lewton


Vorbis decoder written in pure Rust.


cargo run --example player /path/to/your/audio_file.ogg
It will then play back the audio.

If you want to know how to use this crate, look at the examples folder.



# Claxon

https://github.com/ruuda/claxon


A FLAC decoding library in Rust.


Claxon is a FLAC decoder written in pure Rust. It has been fuzzed and verified against the reference decoder for correctness. Its performance is similar to the reference decoder.

Example
The following example computes the root mean square (RMS) of a FLAC file:

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

More examples can be found in the examples directory. For a simple example of decoding a FLAC file to wav with Claxon and Hound, see decode_simple.rs. A more efficient way of decoding requires dealing with a few details of the FLAC format. See decode.rs for an example.