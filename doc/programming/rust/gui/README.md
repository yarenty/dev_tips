---
title: Rust GUI
keywords: [rust, gui, desktop, mobile, webview, tauri, slint, egui, dioxus, iced]
status: reviewed
---

# Rust GUI

Articles in this section cover the Rust UI / app-shell landscape: cross-platform GUI frameworks, the window/event substrates underneath them, web-tech wrappers, macOS-native bindings, mobile-target tooling, and a handful of Linux/macOS desktop niceties.

## What to reach for when

| If you want…                                              | Reach for                                  |
| --------------------------------------------------------- | ------------------------------------------ |
| A dev tool, debug overlay, or quick internal app          | [[egui]]                                   |
| A polished native UI with a designer/dev split (HMI/embedded) | [[slint]]                              |
| Type-safe Elm-architecture apps; Cosmic-desktop-shaped    | [[ised\|iced]]                             |
| React-shaped, one codebase to web/desktop/mobile          | [[dioxus]]                                 |
| Web-tech UI + Rust backend with tiny bundles              | [[programming/rust/gui/tauri\|tauri]]      |
| Wrap an existing URL as a desktop app, no Rust to write   | [[pake]]                                   |
| Window + event loop only (with menus / tray)              | [[tao]] (or upstream `winit`)              |
| Native macOS / iOS UI bindings                            | [[cacao]]                                  |
| One Rust app on iOS *and* Android                         | [[mobile]] (`cargo-mobile2`)               |
| The "Are We GUI Yet?" landscape view                      | [[ui]]                                     |

## Cross-platform GUI frameworks

- [[egui]] — immediate-mode GUI; web + native; killer for dev tools and overlays.
- [[slint]] — declarative `.slint` markup compiled to native; embedded-friendly; dual-licensed.
- [[ised|iced]] — Elm-architecture, type-safe message passing; powers System76's Cosmic desktop. (Note: filename `ised.md` is a historical typo; article is about Iced.)
- [[dioxus]] — React-shaped, signals-based; targets web/desktop/mobile/TUI/SSR from one tree.
- [[ui]] — Rust-GUI landscape index ("Are We GUI Yet?", floem, EWW, Seelen UI, 7GUIs).

## Window / event substrate

- [[tao]] — Tauri team's `winit` fork with menus, tray, file-drop, accelerators baked in.

## Web-tech wrappers / shells

- [[programming/rust/gui/tauri|tauri]] — OS-native webview shell with a Rust backend; Electron alternative.
- [[pake]] — Tauri-based "wrap any URL as a desktop app" CLI. (Note: filename ambiguity — see also `[[tools/security/pake|pake (crypto)]]`.)

## macOS-native

- [[cacao]] — Rust bindings to AppKit (macOS) and UIKit (iOS).
- [[mac_notification_sys]] — narrow `NSUserNotification` wrapper.
- [[macos]] — landing for the Apple Rust target story; the `rustup target add` line.
- [[rust_xcode_plugin]] — Brainium-era Xcode plugin for Rust source-level debugging (largely superseded).

## Mobile

- [[mobile]] — `cargo-mobile2`: project bootstrapper + build runner for iOS and Android.
- [[lipo]] — `cargo-lipo` for fusing multi-arch staticlibs into one fat iOS binary.
- [[matrix]] — Apple bindings of `matrix-rust-sdk` (Matrix chat protocol client SDK).

## Linux desktop niceties

- [[swww]] — animated Wayland wallpaper daemon; Sway / Hyprland / River users.

## Keywords

`#rust` `#gui` `#desktop` `#mobile` `#tauri` `#slint` `#egui` `#dioxus` `#iced`

## See also

- [[../web/README|programming/rust/web]] — Tauri overlaps heavily with the Rust web-frontend story (Yew, Leptos, etc.).
- [[../tui/README|programming/rust/tui]] — when a TUI is the right answer instead of a GUI.
- [[../core/README|programming/rust/core]] — language-level patterns most GUI code touches.
- [[../tooling/README|programming/rust/tooling]] — there is a thinner [[programming/rust/tooling/tauri|tauri]] stub here too.
