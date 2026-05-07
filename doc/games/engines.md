---
title: Rust game engines
main_link: https://arewegameyet.rs/
keywords: [game-engines, rust, bevy, piston, ecs, rendering]
status: reviewed
---

# Rust game engines

**Main link:** <https://arewegameyet.rs/>

## Summary

Quick comparative pointer at the two long-standing Rust game engines: **Bevy** (the modern, ECS-first, hot-reloading, growing-fast option) and **Piston** (the older, more modular, less opinionated option). Both are open-source crates; Bevy is now the default suggestion for new Rust gamedev projects.

## Insight

If you're starting a new Rust game in 2026, reach for **Bevy** unless you have a specific reason not to. It has momentum, an ECS that genuinely teaches you the architecture pattern, a thriving plugin ecosystem (UI, physics, audio, networking), and active discussion on its weekly newsletter. **Piston** is the older "kit of crates" approach — every subsystem is a separate crate you compose. It's still maintained but development is sleepy; pick it if you want minimalism / explicit control over what's pulled in. For tooling, browse `arewegameyet.rs` — the Rust-gamedev community's living portal.

## Similar / related topics

- `macroquad` / `miniquad` — minimal, single-file game frameworks; great for jam-style projects.
- `ggez` — friendlier "LÖVE2D for Rust" wrapper.
- Godot + GDExtension Rust bindings — when you want a full game editor, not just an engine.
- `wgpu`, `winit` — the layer Bevy/Piston build on; useful directly if you want a custom renderer.
- [[zxspectrum]] — Sibling Rust gamedev project (an emulator, but same ecosystem).

## Internal links

- [[zxspectrum]] — Other Rust gamedev work (Z80 emulator).
- [[corewars]] — Same "build a tiny VM/sim in Rust" lineage.

## Keywords

`#game-engines` `#rust` `#bevy` `#piston` `#ecs` `#rendering`

## References / raw notes

### Bevy

- Project site: <https://bevyengine.org/>
- Repo: <https://github.com/bevyengine/bevy>
- Weekly newsletter / "This Week in Bevy": <https://thisweekinbevy.com/>
- Plugin index: <https://bevyengine.org/assets/>

ECS-first, data-driven, supports hot-reload, fast-moving (breaking changes between releases until 1.0). Good docs, excellent example gallery.

### Piston

- crates.io: <https://crates.io/crates/piston>
- "Games made with Piston" gallery: <https://github.com/PistonDevelopers/piston/wiki/Games-Made-With-Piston>
- Repo: <https://github.com/PistonDevelopers/piston>

Modular: a core event/window crate plus a long list of optional sibling crates (`piston2d-graphics`, `piston-image`, …). Slower-moving than Bevy now.

### Where to look next

- **Are We Game Yet?** — community-maintained portal listing engines, libraries, sample projects, and learning resources: <https://arewegameyet.rs/>
- **Rust Game Dev Discord / forum** — most active community hub.
- **`r/rust_gamedev`** subreddit — weekly screenshots and recent updates.
