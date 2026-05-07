---
title: "iOS development on macOS"
main_link: https://developer.apple.com/support/app-store-improvements/
keywords: [ios, apple, app-store, xcode, swiftui, debugging, code-review, mobile]
status: reviewed
---

# iOS development on macOS

**Main link:** <https://developer.apple.com/support/app-store-improvements/>

Docs: <https://developer.apple.com/app-store/review/> · Debugging: <https://developer.apple.com/support/debugging/>

## Summary

Quick-jump landing page for the official Apple developer entry points that matter when you're shipping an iOS app: the App Store improvements / policy updates feed, the App Review guidelines, and Apple's debugging support docs. iOS development is gated through Apple's tooling (Xcode, Instruments, the simulator, and Apple's signing infrastructure), so most workflows ultimately route through `developer.apple.com`.

## Insight

iOS work is unavoidably **macOS-only at the build stage** — Xcode, the iOS Simulator, and the code-signing pipeline don't exist anywhere else. Even if your app source is cross-platform (React Native, Flutter, Tauri Mobile, KMP), the final archive / notarize / upload step needs a Mac, which is the practical reason this article lives in `tools/osx/`.

Two recurring pain points worth bookmarking:

- **App Review** — the guidelines change frequently and rejections are usually about metadata, in-app purchase routing, or privacy disclosures rather than code. Keep the guidelines tab open before submitting.
- **Debugging on device** — provisioning, entitlements, and the sysdiagnose / Console flow have their own learning curve; the debugging support page is the canonical jumping-off point.

If you don't *have* a Mac and want to do CI-style cross-builds (e.g. for a Rust or C library targeting iOS or macOS), see [[osxcross]] for the macOS half. iOS device cross-builds also need the iOS SDK, which has stricter licensing than the macOS SDK.

## Similar / related topics

- [[osxcross]] — cross-compile to macOS from Linux; useful in CI alongside iOS builds.
- [[utm]] — virtualize macOS on macOS for clean test environments (limited use for iOS sim, which needs the host).
- [Fastlane](https://fastlane.tools/) — automation for iOS/Android signing, screenshots, TestFlight, App Store upload.
- [SwiftUI](https://developer.apple.com/xcode/swiftui/) — modern declarative UI framework, the default for new iOS apps.
- [Tauri Mobile](https://v2.tauri.app/start/) — Rust-backed cross-platform mobile shell, an alternative to React Native / Flutter.

## Internal links

- [[osxcross]]
- [[utm]]
- [[osx_tricks]]

## Keywords

`#ios` `#apple` `#xcode` `#swiftui` `#app-store` `#code-review` `#debugging` `#mobile-dev`

## References / raw notes

### Apple developer entry points

- App Store improvements / policy updates: <https://developer.apple.com/support/app-store-improvements/>
- App Review guidelines: <https://developer.apple.com/app-store/review/>
- Debugging support: <https://developer.apple.com/support/debugging/>
