---
title: Misc Rust — kitchen-sink section
keywords: [rust, misc, audio, embedded, raspberry-pi, windows, charts, fun]
status: reviewed
---

# Misc Rust — kitchen-sink section

This is a genuine kitchen-sink section: things that are useful Rust crates / topics but didn't have a natural home elsewhere in `programming/rust/`. Most of the entries cluster into three real themes plus a small whimsy + reference tail; group them mentally rather than treating this as a flat list.

## Audio

End-to-end Rust audio: device I/O → codec → playback.

- [[audio]] — **start here**, ecosystem overview + decision tree (cpal / symphonia / rodio at the centre).
- [[symphonia]] — modern unified pure-Rust media demuxer + codec (FLAC / MP3 / AAC / ALAC / Vorbis / WAV / OGG / WebM).
- [[rodio]] — high-level "play this sound" library; sits on `cpal` + Symphonia.
- [[hound]] — pure-Rust WAV reader/writer (small scope, very stable).
- [[claxon]] — pure-Rust FLAC decoder.
- [[lewton]] — pure-Rust Vorbis (.ogg) decoder.
- [[minimp3]] — Rust bindings to the C minimp3 MP3 decoder (now flagged "not recommended for new projects" — prefer Symphonia or `nanomp3`).

**Decision shortcuts:**

| You want… | Reach for |
|---|---|
| Play a sound from a file or buffer | [[rodio]] |
| Decode any common audio file | [[symphonia]] |
| Just write a WAV (recording, transcoding) | [[hound]] |
| Decode only FLAC | [[claxon]] |
| Decode only Vorbis (.ogg) | [[lewton]] |
| Decode only MP3 | [[symphonia]] (preferred) or [[minimp3]] |
| Talk to the OS audio device directly | [`cpal`](https://github.com/RustAudio/cpal) |

## Embedded

Rust on microcontrollers and on Linux-class single-board computers.

- [[awesome_embedded_in_rust]] — entry-point reading list (Rust Embedded WG).
- [[embassy]] — async-first embedded framework + HALs + executor.
- [[rtic]] — Real-Time Interrupt-driven Concurrency; static-priority alternative to Embassy.
- [[raspberry_pi]] — both the Linux-class Pi (rppal, cross-compile) and the Pico (RP2040 / RP2350) story.

**RTIC vs Embassy:** RTIC is interrupt-priority + Stack Resource Policy, deterministic latency, hard real-time. Embassy is async/await on a no-`std` cooperative executor, I/O-rich, low power. Both can coexist in one binary.

## Whimsy / fun

Small Rust projects that exist mostly for joy — useful as readable end-to-end Rust examples.

- [[daktilo]] — typewriter sound effects on keypress (orhun).
- [[fun]] — `bats` and other "Rust crates that exist for joy" landing.

## Reference / curation

Curated landing pages — pick the right crate, then jump out to the canonical home.

- [[algorithms]] — pointer at `TheAlgorithms/Rust` (read-it-don't-depend-on-it).
- [[charts]] — Rust plotting / charting landscape (plotters / charming / plotly / egui_plot).
- [[windows]] — `windows-rs`, Microsoft's official Rust Windows API projection.

## Cross-section see-also

- [[../../../iot/README|iot]] — IoT / Drogue / Akri / device-fleet topics (the cloud side of `embassy` + `raspberry_pi`).
- [[../../../visualization/README|visualization]] — vault-wide viz section (parent of [[../../../visualization/plotters|plotters]]).
- [[../../../funny/README|funny]] — vault-wide whimsy section (Brainfuck, sleep-sort, esolangs).
- [[../../../kubernetes/k3s|k3s]] — for Pi-cluster Kubernetes deployments.
- [[../gui/README|Rust GUI]] — for Windows GUI toolkit choices that pair with `windows-rs`.

## Keywords

`#rust` `#misc` `#audio` `#embedded` `#raspberry-pi` `#windows` `#charts` `#fun`
