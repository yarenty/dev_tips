---
title: app_builders — Awesome App and the Rust desktop-packaging story
main_link: https://awesomeapp.org/
keywords: [app-builders, rust, awesome-app, tauri, surrealdb, packaging, desktop]
status: reviewed
review_date: 2026/05/03
---

# app_builders — Awesome App and the Rust desktop-packaging story

**Main link:** <https://awesomeapp.org/>

## Summary

[`Awesome App`](https://awesomeapp.org/) is a Rust template by Jeremy Chone for building desktop applications using [[tauri|Tauri]], [[../data/surrealdb|SurrealDB]] (embedded), and Native Web Components on the front end. It scaffolds a working app with backend Rust commands, an IPC bridge, an SQL-shaped local store, a Postcss/Rollup/TypeScript front-end pipeline, and the build wiring to produce a signed installer per OS. Use it as a starting template, not as a framework you depend on.

## Insight

The "build a desktop app in Rust" question splits into two: **the runtime** (which UI toolkit?) and **the packaging** (how do I ship a `.dmg` / `.msi` / `.AppImage` / `.deb` per platform that users can double-click?).

For the runtime, see the canonical [[../gui/tauri|gui/tauri]] (web-tech UI) or [[../gui/dioxus|dioxus]] / [[../gui/slint|slint]] / [[../gui/egui|egui]] for native-Rust UIs.

For packaging — the actual subject of "app builders" — the pieces are:

- **`cargo-bundle`** — the original "package my Rust binary as a platform-native installer" tool. Limited maintenance.
- **`cargo-tauri`** (Tauri's CLI) — the most-used answer if you went the web-tech UI route. Handles per-OS packaging (Apple notarisation, Windows code signing, Linux AppImage/deb/rpm) within the Tauri workflow.
- **`cargo-deb`** / **`cargo-rpm`** / **`cargo-generate-rpm`** — single-distro Linux packaging for a plain Rust binary.
- **`cargo-binstall`** — installs precompiled binaries from GitHub releases (the "I don't want to compile your CLI from source" answer for end users).
- **`cargo-zigbuild`** / **`cross`** — cross-compile from one OS to another. zigbuild is currently the most-loved approach because Zig handles the cross-libc problem well.
- **`cargo-dist`** (axodotdev) — opinionated end-to-end release CI: builds for every triple, signs, uploads to GitHub releases, generates an installer script. The most modern story for CLI tools.

`Awesome App` itself is the **Tauri + SurrealDB + native-WC opinion** for desktop apps. If that stack matches you, it's a useful starting template. If you want different DB / UI choices, lift the packaging wiring and ignore the rest.

**When to reach for what**:

- Shipping a CLI? → `cargo-dist` + `cargo-binstall`.
- Shipping a desktop app with web-tech UI? → `cargo-tauri` (with optional `Awesome App` template).
- Shipping a Linux-only daemon? → `cargo-deb` or `cargo-generate-rpm`.
- Cross-compiling for ARM / Windows / macOS from Linux CI? → `cargo-zigbuild` or `cross`.

## Similar / related topics

- [[../gui/tauri|gui/tauri]] — canonical Tauri framework article (UI-toolkit angle).
- [[../data/surrealdb|surrealdb]] — embedded DB used by Awesome App template.
- [`cargo-dist`](https://opensource.axo.dev/cargo-dist/) — modern release-pipeline tool.
- [`cargo-binstall`](https://github.com/cargo-bins/cargo-binstall) — install precompiled crates.
- [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) — cross-compile via Zig.
- [`cross`](https://github.com/cross-rs/cross) — Docker-based cross-compilation.

## Internal links

- [[README]] — tooling section landing.
- [[tauri]] — the dev-tooling angle on Tauri.
- [[../gui/tauri|gui/tauri]] — canonical Tauri framework article.
- [[../data/surrealdb|surrealdb]] — embedded DB in the Awesome App template.

## Keywords

`#app-builders` `#rust` `#awesome-app` `#tauri` `#packaging` `#desktop` `#cargo-dist`

## References / raw notes

- Site: <https://awesomeapp.org/>
- Code-review video: <https://www.youtube.com/watch?v=BY_ZjPGqJJk> — Jeremy Chone walks through the template (Rust + Tauri + SurrealDB + Native Web Components):
  - 00:00 — Intro to Awesome App
  - 01:56 — Create the App
  - 02:58 — Code Overview
  - 04:04 — Backend Code Review
  - 26:31 — Frontend Code Review
  - 40:04 — Setup files (Postcss, Rollup, TypeScript, and `Awesome.toml`)
