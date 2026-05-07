---
title: cargo-mobile / cargo-mobile2
main_link: https://github.com/tauri-apps/cargo-mobile2
keywords: [mobile, cargo-mobile, cargo-mobile2, rust, ios, android, tauri, bevy]
status: reviewed
review_date: 2026/05/03
---

# cargo-mobile / cargo-mobile2

**Main link:** <https://github.com/tauri-apps/cargo-mobile2>

## Summary

`cargo-mobile2` is the Tauri team's actively-maintained fork of the original `cargo-mobile` (BrainiumLLC). It answers the question *"how do I use Rust on iOS and Android?"* by generating Xcode and Android Studio project files, building and running on a connected device or simulator, and shipping a few template packs (wry, egui, winit) so a `cargo mobile init` produces a working cross-platform skeleton in seconds. Tauri now uses cargo-mobile2 as a library dependency rather than as a separate CLI.

## Insight

For new projects, **use `cargo-mobile2`, not the original `cargo-mobile`** — the Brainium repo is no longer maintained and has known build breakage on recent Rust releases, while cargo-mobile2 supports macOS, Linux, *and* Windows (only iOS targets are still macOS-only, an Apple licensing constraint). The mental model: cargo-mobile2 is a *project bootstrapper plus build runner*, not a renderer or framework — it knits together your existing GUI choice (Tauri, wry, egui, winit + custom, Bevy) with the platform-native build pipeline (Xcode for iOS, Gradle/AGP for Android), giving you `cargo apple run` and `cargo android run` shortcuts. If you've adopted Tauri 2, you mostly *don't* invoke cargo-mobile2 directly — `tauri ios dev` / `tauri android dev` is the user-facing wrapper. The Bevy template was broken at the time of the upstream README; check current status before reaching for it.

```shell
cargo install --git https://github.com/tauri-apps/cargo-mobile2
mkdir myapp && cd myapp
cargo mobile init        # interactive: pick a template (wry / egui / winit)
cargo apple run          # iOS device or simulator
cargo android run        # Android device
```

## Similar / related topics

- [[programming/rust/gui/tauri|tauri]] — Tauri 2 mobile uses cargo-mobile2 under the hood; preferred frontend.
- [[slint]] — Slint has its own mobile support path; not via cargo-mobile2.
- `xbuild` (`x-rs`) — independent cross-platform Rust build tool spanning iOS/Android/Linux/macOS/Windows.
- `cargo-apk` / `ndk-build` — lower-level Android-only Rust packaging.
- [[lipo]] — fat-binary glue if you build raw staticlibs instead of going through this.

## Internal links

- [[programming/rust/gui/tauri|tauri]]
- [[macos]]
- [[lipo]]
- [[rust_xcode_plugin]]
- [[../README]]

## Keywords

`#cargo-mobile` `#cargo-mobile2` `#rust` `#ios` `#android` `#tauri` `#mobile`

## References / raw notes

- Maintained fork: <https://github.com/tauri-apps/cargo-mobile2>
- Original (deprecated): <https://github.com/BrainiumLLC/cargo-mobile>
- Tauri mobile guide: <https://tauri.app/develop/#mobile>

### Quick-start

```shell
cargo install --git https://github.com/tauri-apps/cargo-mobile2
cargo mobile update      # pull latest later
cargo mobile init        # bootstrap a project
```

Template packs available at init time include `wry` (minimal Tauri webview app) and `egui` (egui + winit + wgpu, based on the agdk-egui example).

### Android logging

`cargo android run` builds, installs, runs, and tails device logs. Verbosity:

- default: `warn` + `error`
- `-v` / `-vv`: more verbose
- `--filter` / `-f`: an explicit Android log level (`debug`, etc.)

If you wire Rust's `log` to Android via the `android_logger` crate, Rust `trace` maps to Android `verbose`.
