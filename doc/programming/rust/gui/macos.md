---
title: macOS / iOS Rust targets
main_link: https://doc.rust-lang.org/rustc/platform-support.html
keywords: [macos, ios, rust, rustup, targets, apple-silicon, lipo, xcode]
status: reviewed
---

# macOS / iOS Rust targets

**Main link:** <https://doc.rust-lang.org/rustc/platform-support.html>

## Summary

Landing page for the macOS- and iOS-flavoured Rust ecosystem in this vault. The one-liner most pages need is the `rustup target add` for Apple's five interesting triples (Apple-silicon device + simulator on Apple-silicon + intel sim + Apple-silicon mac + intel mac); from there the per-piece articles cover bindings ([[cacao]]), notifications ([[mac_notification_sys]]), build glue ([[lipo]] / [[mobile]] / [[rust_xcode_plugin]]), and the iOS Matrix client SDK ([[matrix]]).

## Insight

In practice the only thing you *always* need is the `rustup target add` line below — everything else depends on what you're building. If you want a native macOS UI, see [[cacao]] (AppKit/UIKit wrapper) or use [[programming/rust/gui/tauri|tauri]] for a webview shell. If you want one Rust app on iOS *and* Android, see [[mobile]] (`cargo-mobile2`). If you only need notifications, [[mac_notification_sys]] is the smallest possible dependency. The `rust-xcode-plugin` Brainium-era story has largely been superseded by `cargo-mobile2` and Tauri's mobile pipeline; the [[rust_xcode_plugin]] page is kept for historical context.

```shell
# Cover all current Apple Rust targets
rustup target add \
    aarch64-apple-ios \
    aarch64-apple-ios-sim \
    x86_64-apple-ios \
    aarch64-apple-darwin \
    x86_64-apple-darwin
```

| Triple                     | What it's for                                  |
| -------------------------- | ---------------------------------------------- |
| `aarch64-apple-darwin`     | macOS on Apple Silicon (M1/M2/M3/M4).          |
| `x86_64-apple-darwin`      | macOS on Intel Macs.                           |
| `aarch64-apple-ios`        | iOS on real device (all modern iPhones/iPads). |
| `aarch64-apple-ios-sim`    | iOS Simulator on an Apple-Silicon Mac.         |
| `x86_64-apple-ios`         | iOS Simulator on an Intel Mac.                 |

## Similar / related topics

- [[cacao]] — Rust bindings to AppKit / UIKit.
- [[mac_notification_sys]] — narrow `NSUserNotification` wrapper.
- [[lipo]] — fuse multi-arch staticlibs for iOS.
- [[mobile]] / [[rust_xcode_plugin]] — Xcode + Android Studio integration.
- [[matrix]] — iOS bindings of the Matrix Rust SDK.

## Internal links

- [[cacao]]
- [[mac_notification_sys]]
- [[lipo]]
- [[mobile]]
- [[rust_xcode_plugin]]
- [[matrix]]
- [[../README]]

## Keywords

`#macos` `#ios` `#rust` `#rustup` `#targets` `#apple-silicon`

## References / raw notes

- Rust platform support matrix: <https://doc.rust-lang.org/rustc/platform-support.html>
- Apple-side overview: <https://developer.apple.com/xcode/>

```shell
rustup target add aarch64-apple-ios aarch64-apple-ios-sim x86_64-apple-ios aarch64-apple-darwin x86_64-apple-darwin
```
