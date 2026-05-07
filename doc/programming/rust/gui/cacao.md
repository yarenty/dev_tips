---
title: Cacao
main_link: https://github.com/ryanmcgrath/cacao
keywords: [cacao, rust, appkit, uikit, macos, ios, ffi]
status: reviewed
---

# Cacao

**Main link:** <https://github.com/ryanmcgrath/cacao>

## Summary

Cacao provides safe(ish) Rust bindings for AppKit on macOS (beta) and UIKit on iOS / tvOS (alpha). Maintained by Ryan McGrath, it tries to feel familiar to anyone who has written Cocoa in Swift or Objective-C, while papering over the ownership-model mismatch between Rust and the Apple frameworks. Use it when you want a *native* mac/iOS UI — `NSWindow`, `NSToolbar`, real `NSTableView`, system menus — written in Rust, rather than a webview or a custom-drawn canvas.

## Insight

Reach for Cacao when "feels like a Mac app" is part of the requirement and a webview-shell (Tauri) or immediate-mode canvas (egui) is the wrong shape. The trade-off is that you are still writing AppKit/UIKit, just spelled in Rust: you need to know the framework, and the Rust safety story is necessarily best-effort because the underlying objects mutate via Objective-C runtime calls. The main competitor in the same niche is [`objc2`](https://github.com/madsmtm/objc2) + the `objc2-app-kit` / `objc2-ui-kit` crates, which are lower-level but more thoroughly maintained — Cacao gives you opinionated wrappers, objc2 gives you raw bindings you build on top of. For pure macOS notifications only, the much narrower [[mac_notification_sys]] is enough.

## Similar / related topics

- objc2 — auto-generated, actively-maintained Objective-C runtime bindings; the lower-level alternative.
- [[mac_notification_sys]] — narrow wrapper for `NSUserNotification` only.
- [[programming/rust/gui/tauri|tauri]] — webview-shell alternative if you can tolerate HTML/JS for UI.
- winit / [[tao]] — only handle window + event loop; no native widgets.
- SwiftUI / Swift — the official Apple path; consider `swift-bridge` if you want Rust core + Swift UI.

## Internal links

<!-- reviewed -->
- [[macos]]
- [[mac_notification_sys]]
- [[mobile]]
- [[programming/rust/gui/tauri|tauri]]
- [[../README]]

## Keywords

`#cacao` `#rust` `#appkit` `#uikit` `#macos` `#ios` `#ffi`

## References / raw notes

This library provides safe Rust bindings for AppKit on macOS (beta quality, fairly usable) and UIKit on iOS/tvOS (alpha quality, see repo). It tries to do so in a way that, if you've done programming for the framework before (in Swift or Objective-C), will feel familiar. This is tricky in Rust due to the ownership model, but some creative coding and assumptions can get us pretty far.

- Repo: <https://github.com/ryanmcgrath/cacao>
- Lower-level alternative: <https://github.com/madsmtm/objc2>
