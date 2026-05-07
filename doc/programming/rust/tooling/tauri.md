---
title: tauri (dev-tooling angle) — `cargo tauri` workflow
main_link: https://tauri.app/
keywords: [tauri, rust, cargo-tauri, desktop, mobile, packaging, ipc, webview]
status: reviewed
---

# tauri (dev-tooling angle) — `cargo tauri` workflow

**Main link:** <https://tauri.app/>

> **This article covers the dev-tooling angle on Tauri** (the `cargo tauri` CLI, install workflow, project setup). For the framework architecture (webview vs Electron, IPC bridge, Tauri 1.x vs 2.x mobile story, alternatives like Wails / Neutralino / pake), see the canonical [[../gui/tauri|programming/rust/gui/tauri]].

## Summary

Tauri is a Rust framework for building desktop and (since 2.x) mobile apps where the UI is web tech and the backend is a Rust binary; it uses the OS-native webview rather than bundling Chromium, giving small (~5–15 MB) bundles versus Electron's 80–150 MB. From a *tooling* perspective, the day-to-day surface is the `cargo tauri` (or `npm run tauri`) CLI, which scaffolds, dev-runs, and packages an app. See the canonical framework overview at [[../gui/tauri|gui/tauri]].

## Insight

The dev-tooling story is:

1. **Prerequisites**: Rust toolchain + system deps (WebKitGTK on Linux, Microsoft Edge WebView2 on Windows — preinstalled on Win11; nothing extra on macOS). Tauri's [getting-started page](https://tauri.app/start/prerequisites/) lists the exact apt/dnf/pacman lines.

2. **Scaffold**: `cargo install create-tauri-app && cargo create-tauri-app` (interactive wizard, picks a JS framework). The result is a hybrid Rust + JS workspace with `src-tauri/` for the Rust side and your JS framework's directory for the front-end.

3. **Dev loop**: `cargo tauri dev` runs both sides with hot-reload (Vite/Webpack on the front, `cargo watch` on the back). `cargo tauri build` produces a packaged app per platform.

4. **Packaging**: handled by `cargo tauri build` — produces `.dmg` + `.app` on macOS, `.msi` + `.exe` on Windows, `.AppImage` + `.deb` on Linux. Apple notarisation and Windows code signing are configured in `src-tauri/tauri.conf.json`.

5. **Mobile (Tauri 2.x+)**: `cargo tauri ios init` / `cargo tauri android init` then `cargo tauri ios dev`. Requires the platform SDKs locally (Xcode for iOS, Android Studio for Android) and uses `cargo-mobile2` under the hood. See [[../gui/mobile|gui/mobile]] for the cross-platform Rust-mobile picture.

**Common gotchas (tooling-specific)**:

- **WebKitGTK version skew on Linux**: pin a known-good version in CI.
- **Apple notarisation** requires a paid Developer ID and an app-specific password; budget time for the first run.
- **Bundle ID ambiguity**: Tauri 1.x and 2.x project layouts differ; mixing instructions across versions silently breaks.
- **First `cargo tauri build`** can take 5–10 minutes on a cold cache.

For everything else (architecture, IPC, alternatives, Sidecar pattern, security trade-offs), see [[../gui/tauri|gui/tauri]].

## Similar / related topics

- [[../gui/tauri|gui/tauri]] — the canonical framework article.
- [[app_builders]] — broader Rust desktop-packaging story (`cargo-tauri` is one of several options).
- [[../gui/mobile|gui/mobile]] — `cargo-mobile2` and the cross-platform Rust-mobile picture.
- Electron / Wails / Neutralino — non-Rust alternatives (covered in `gui/tauri`).

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[../gui/tauri|gui/tauri]] — canonical framework article (read this for architecture).
- [[app_builders]] — broader desktop-packaging context.
- [[../gui/mobile|gui/mobile]] — Tauri 2.x mobile substrate.

## Keywords

`#tauri` `#cargo-tauri` `#rust` `#desktop` `#mobile` `#packaging` `#webview`

## References / raw notes

- Site: <https://tauri.app/>
- v2 getting started: <https://tauri.app/start/>
- v1 setup (legacy): <https://tauri.app/v1/guides/getting-started/setup>
- Install scaffolder: `cargo install create-tauri-app`.
