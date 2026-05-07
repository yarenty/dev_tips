---
title: Tauri
main_link: https://tauri.app/
keywords: [tauri, rust, webview, electron-alternative, desktop, mobile, wry, tao]
status: reviewed
---

# Tauri

**Main link:** <https://tauri.app/>

## Summary

Tauri is a Rust framework for building small, fast desktop and mobile apps where the UI is web tech (any JS/TS framework that compiles to HTML/CSS/JS) and the backend is a Rust binary that exposes commands to the front-end via an IPC bridge. It uses the **OS-native webview** (WKWebView on macOS/iOS, WebView2 on Windows, WebKitGTK on Linux, Android System WebView on Android) instead of bundling Chromium, so a typical Tauri bundle is ~5–15 MB versus Electron's 80–150 MB. Tauri 1.x covers desktop; Tauri 2.x added iOS and Android targets (GA late 2024). Window management goes through [[tao]]; the WebView abstraction is the sibling crate `wry`.

> Filename ambiguity: `tauri.md` exists both at `programming/rust/gui/tauri.md` (this file — the canonical Tauri article) and `programming/rust/tooling/tauri.md` (a much thinner stub). When linking from outside `gui/`, prefer `[[programming/rust/gui/tauri|tauri]]`.

## Insight

Reach for Tauri when (a) your team already knows web tech and you'd rather keep that productivity than learn a Rust UI toolkit, and (b) bundle size and per-platform fit matter more than a *single, identical* Chromium runtime. The latter is the main trade-off: the OS webview means each platform is *almost* the same but not quite — modern WebKit and Chromium-Edge cover most of the spec, but you'll occasionally hit a platform-specific CSS or JS quirk that Electron would have masked. Compared to alternatives: **Electron** is heavier and offers Node APIs in the renderer (Tauri keeps the Rust side strictly behind an IPC boundary, which is also a security benefit); **Wails** is the same shape with Go instead of Rust; **Neutralino** is even thinner; **[[pake]]** is essentially "default Tauri config + a CLI for wrapping URLs". For the *Sidecar* pattern (shipping a separate compiled binary your Rust side spawns at runtime — e.g. embedding `ffmpeg`, a Python interpreter, or a model server) Tauri 2 has the cleanest story of any of them.

## Similar / related topics

- Electron — the JS-everything alternative; bigger bundles, identical Chromium.
- Wails (Go) — same architectural shape, Go backend.
- Neutralino — lighter still; uses OS webview but smaller framework footprint.
- [[pake]] — opinionated default Tauri config + CLI for "wrap any URL".
- [[dioxus]] / [[slint]] — alternatives if you want to avoid web tech entirely.
- [[tao]] / `wry` — the windowing + WebView crates underneath Tauri.
- `cargo-mobile2` (see [[mobile]]) — what Tauri 2 mobile uses internally.

## Internal links

<!-- reviewed -->
- [[tao]]
- [[pake]]
- [[mobile]]
- [[dioxus]]
- [[slint]]
- [[../README]]
- [[../web/README|web]]

## Keywords

`#tauri` `#rust` `#webview` `#electron-alternative` `#desktop` `#mobile` `#wry`

## References / raw notes

- Site: <https://tauri.app/>
- Repo: <https://github.com/tauri-apps/tauri>
- Architecture: <https://github.com/tauri-apps/tauri/blob/dev/ARCHITECTURE.md>
- WebView abstraction: <https://github.com/tauri-apps/wry>
- Window/event substrate: <https://github.com/tauri-apps/tao>
- Mobile bootstrap: <https://github.com/tauri-apps/cargo-mobile2>

### How the pieces fit

The UI is whatever web framework you like (React, Vue, Svelte, Solid, Lit, vanilla — anything that compiles to HTML/JS/CSS). At runtime that bundle is loaded into the OS-native WebView via `wry`, inside a window managed by [[tao]]. Your Rust side registers `#[tauri::command]` functions that the JS side invokes through a typed IPC bridge.

```text
[ JS UI ] ── tauri ipc ──► [ Rust commands ]
   ▲                              │
   │ wry (system WebView)         │ tao (window/event loop)
   └──────────── window ──────────┘
```

### Mobile (Tauri 2)

Tauri 2 ships first-class iOS and Android targets via `cargo-mobile2`. From a Tauri 2 project root:

```shell
# install Tauri 2 CLI (npm or cargo)
cargo install tauri-cli --version "^2"

# bootstrap mobile targets (interactive)
cargo tauri ios init
cargo tauri android init

# run on device / simulator
cargo tauri ios dev
cargo tauri android dev
```

Building for iOS still requires macOS + Xcode; Android can be built from any host with the Android SDK/NDK installed. See [[mobile]] for the cargo-mobile2 background.
