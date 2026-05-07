---
title: tao
main_link: https://github.com/tauri-apps/tao
keywords: [tao, rust, winit, fork, window, event-loop, tauri]
status: reviewed
---

# tao

**Main link:** <https://github.com/tauri-apps/tao>

## Summary

`tao` is the Tauri team's fork of `winit`, the de-facto Rust cross-platform window-creation and event-loop crate. tao tracks winit closely but adds the bits that desktop applications (rather than games / engines) need first-class: native menus, system tray, file-drop events, modifier-key handling, accelerator keys, and a few macOS niceties. It is the windowing substrate beneath Tauri itself and ships with Tauri 1.x and 2.x.

## Insight

Reach for tao when you're building a Rust *application* (not a game) directly on top of a window+event substrate and you find yourself needing menus, a tray icon, or other "this is an app, not a renderer" features that vanilla winit doesn't ship. If you're already building on Tauri or Wry you're already using tao indirectly. The trade-off is that tao is a fork: upstream winit eventually adds many of these features (the gap has narrowed in 2024+), and tao's API surface diverges in places, so portability between the two is not free. For pure renderer work — game engines, custom canvas apps — winit remains the default; tao is for things that want an OS chrome.

## Similar / related topics

- `winit` — upstream cross-platform window/event crate; the thing tao forks.
- `wry` — Tauri's WebView wrapper; commonly paired with tao.
- `tray-icon` / `muda` — the tao-team's standalone tray and menu crates (factored out so winit users can use them too).
- [[programming/rust/gui/tauri|tauri]] — the framework that uses tao + wry as its substrate.
- `glutin`, `softbuffer` — companion crates for OpenGL / software-rendered surfaces on winit/tao.

## Internal links

- [[programming/rust/gui/tauri|tauri]]
- [[slint]]
- [[egui]]
- [[../README]]

## Keywords

`#tao` `#rust` `#winit-fork` `#window` `#event-loop` `#tauri`

## References / raw notes

- Repo: <https://github.com/tauri-apps/tao>
- Crate / docs: <https://docs.rs/tao>
- Upstream winit: <https://github.com/rust-windowing/winit>

> tao is a cross-platform application window creation and event-loop management library — Tauri's fork of winit with menu, tray, file-drop, and accelerator extensions for desktop applications.
