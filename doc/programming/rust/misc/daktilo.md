---
title: Daktilo — typewriter sound effects for your keyboard
main_link: https://github.com/orhun/daktilo
keywords: [daktilo, rust, typewriter, sound-effects, orhun, fun]
status: reviewed
---

# Daktilo — typewriter sound effects for your keyboard

**Main link:** <https://github.com/orhun/daktilo>

## Summary

`daktilo` ("typewriter" in Turkish) is a small Rust CLI by [orhun](https://github.com/orhun) that listens to your keyboard and plays typewriter-style sound effects in response. Different keys can trigger different samples (regular key, space, return, backspace), and the sound presets are configurable. It is a personality / vibe tool — orhun also makes [`git-cliff`](https://github.com/orhun/git-cliff), [`gpg-tui`](https://github.com/orhun/gpg-tui), [`kmon`](https://github.com/orhun/kmon), and [`menyoki`](https://github.com/orhun/menyoki).

## Insight

This is firmly in the *whimsy* category: it is genuinely useful for streamers, screencast recorders, and people who like the auditory feedback of a Selectric while writing prose; it is genuinely *not* useful for production work (the audio thread can interact poorly with focus changes, and constant click-clack will distract people in shared spaces). Under the hood it pairs a global keyboard hook (which usually means accessibility / input-monitoring permissions) with [[rodio]] for playback. Treat it as a fun, well-engineered example of a small Rust CLI that does one cute thing well — a good showcase if you want to read the source of an end-to-end audio-driven Rust app.

Other "Rust + sound for fun" projects in similar territory: [`bongo-cat-rs`](https://github.com/Skirlax/bongo-cat-rs) (animated mascot reacts to keys), [`klack`](https://github.com/nathom/klack) (mechanical-keyboard sounds for non-mechanical keyboards, written in Python), and the broader category of "ambient productivity sounds."

## Similar / related topics

- [[rodio]] — the playback library Daktilo uses underneath.
- [`klack`](https://github.com/nathom/klack) — Python equivalent for mechanical-keyboard simulation.
- [`git-cliff`](https://github.com/orhun/git-cliff) — orhun's most popular project; changelog generator.
- [`menyoki`](https://github.com/orhun/menyoki) — orhun's screen-recorder.
- [[../../../funny/README|Funny / esoteric programming]] — adjacent whimsy section.

## Internal links

- [[audio]] — Rust audio ecosystem overview (Daktilo lives at the playback layer).
- [[rodio]] — direct dependency for playing sound samples.
- [[fun]] — sibling whimsy entry in this section.
- [[../../../funny/README|funny]] — vault-wide fun/esoteric section.

## Keywords

`#daktilo` `#rust` `#typewriter` `#sound-effects` `#cli` `#fun` `#orhun`

## References / raw notes

- Repo: <https://github.com/orhun/daktilo>
- Author: orhun (also `git-cliff`, `gpg-tui`, `kmon`, `menyoki`).
- Behaviour: typewriter sound effect on keypress, configurable per-key.
