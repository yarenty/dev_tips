---
title: rustzx — ZX Spectrum emulator in Rust
main_link: https://github.com/rustzx/rustzx
keywords: [zx-spectrum, emulator, rust, z80, retro, sdl2]
status: reviewed
---

# rustzx — ZX Spectrum emulator in Rust

**Main link:** <https://github.com/rustzx/rustzx>

## Summary

`rustzx` is a Sinclair ZX Spectrum 48K / 128K emulator written in Rust, with cross-platform desktop frontends (SDL2, web via wasm). It implements a Z80 CPU core, the Spectrum's ULA video/audio, and tape (`.tap` / `.tzx`) and snapshot (`.sna`) loading. Lightweight, stand-alone, no external dependencies beyond SDL2.

## Insight

The interesting part for vault use is **not** "play retro games" — it's the source. `rustzx` is a clean, modestly-sized example of how to write an instruction-by-instruction CPU emulator in Rust: opcode dispatch via giant `match` (no macro magic), separate frontend / core split so you can swap SDL2 for Wasm or a TUI, and tape-format parsing as a pluggable layer. If you've ever wanted to build a Game Boy / NES / 6502 emulator and wondered where to start, read this code first; the Z80 is small enough to fit in your head and the patterns here transfer directly.

For actual play, fine — load any `.tap` from <https://worldofspectrum.org> and you're 30 seconds away from *Manic Miner*.

## Similar / related topics

- **rustyboy / mooneye-gb / sameboy** — same pattern applied to Game Boy emulation in Rust / C.
- **`fearless_NES`** — NES emulator in Rust; another good study target.
- **MAME** — the giant catch-all multi-system emulator; vastly more code, less digestible.
- **Z80 ISA reference** — <http://www.z80.info/> — the canonical instruction-set documentation.
- [[corewars]] — Sibling "build a tiny VM/sim" article (Redcode is a *much* simpler ISA than Z80).

## Internal links

- [[engines]] — Other Rust gamedev work (engines rather than emulators).
- [[corewars]] — Sibling tiny-VM article.

## Keywords

`#zx-spectrum` `#emulator` `#rust` `#z80` `#retro` `#sdl2`

## References / raw notes

- Repo: <https://github.com/rustzx/rustzx>
- World of Spectrum tape archive: <https://worldofspectrum.org>
- Z80 ISA reference: <http://www.z80.info/>

### What's inside the repo (quick orientation)

- `core/` — Z80 CPU + ULA emulation (no I/O dependencies).
- `frontends/sdl2/` — SDL2 desktop frontend.
- `frontends/web/` — wasm-pack frontend; runs in a browser.
- Tape/snapshot loaders are pluggable into the core.
