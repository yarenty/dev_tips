---
title: "Pake — turn any webpage into a tiny desktop app"
main_link: https://github.com/tw93/Pake
keywords: [pake, tauri, desktop-app, web-to-app, electron-alternative, cli, packaging, rust]
status: reviewed
---

# Pake — turn any webpage into a tiny desktop app

**Main link:** <https://github.com/tw93/Pake>

Releases: <https://github.com/tw93/Pake/releases> · Tauri prerequisites: <https://v2.tauri.app/start/prerequisites/>

> **Filing note:** this article lives under `tools/security/` for historical reasons — it really has nothing to do with security. It's a desktop-packaging tool. Treat the path as legacy and use the wikilink `[[tools/security/pake|pake]]` (the `programming/rust/gui/pake.md` sibling is still a stub).

## Summary

Pake is a CLI by [@tw93](https://github.com/tw93) that wraps any URL into a [Tauri](https://tauri.app/)-based desktop application with one command. The resulting bundles are roughly **20× smaller than equivalent Electron apps** (around 5 MB) and start fast because they reuse the OS-native webview instead of shipping Chromium. Supports macOS, Windows, and Linux. The project also publishes ready-made bundles for popular sites (ChatGPT, DeepSeek, YouTube Music, Excalidraw, etc.) on the Releases page.

## Insight

Pake is for the case where you want a website to feel like an app — its own dock icon, its own window, its own keyboard shortcuts, no browser chrome — without actually writing a desktop app. Think "ChatGPT in a Cmd-Tab-able window" rather than "rebuild Slack from scratch".

How it compares:

- **vs Electron** — Pake is dramatically smaller and lighter because it doesn't ship a browser. The trade-off is that you're at the mercy of the OS webview (WebKit on macOS, WebView2 on Windows, WebKitGTK on Linux), so cross-OS rendering quirks are real.
- **vs raw Tauri** — Pake *is* Tauri underneath, with all the boilerplate (Cargo project, build config, icons, signing) replaced by `pake <url> --name Foo`. If you need to actually call Rust from your "app", drop down to Tauri proper.
- **vs PWA / "Install as app"** — modern Chrome / Edge can install any site as a standalone window with one click, for free, with no build step. Reach for Pake when you want a real installer artefact you can hand someone, custom window chrome, native keyboard shortcuts, or distribution outside the browser.
- **vs Nativefier** — same idea, older project, but Nativefier is Electron-based and produces the larger bundles Pake explicitly set out to avoid.

Honest gotchas: any site that aggressively relies on Chromium-only APIs may misbehave under Safari/WebKitGTK. Auto-updates, code signing, and notarisation are still on you (or on Tauri's tooling). And anything you ship is still just someone else's website wrapped — if they break their layout, your "app" breaks too.

## Similar / related topics

- [Tauri](https://tauri.app/) — the underlying framework; reach for it directly when you need real native code.
- [Electron](https://www.electronjs.org/) — the heavyweight incumbent Pake is a deliberate reaction against.
- [Nativefier](https://github.com/nativefier/nativefier) — older Electron-based equivalent.
- [PWAs](https://web.dev/progressive-web-apps/) — browser-installed standalone web apps; the zero-build option.
- [WebViewApp / Wails](https://wails.io/) — Go alternative that also reuses the OS webview.

## Internal links

- [[fonts]]
- [[react]]
- [[ios]]

## Keywords

`#pake` `#tauri` `#desktop-app` `#web-to-app` `#electron-alternative` `#packaging` `#rust` `#tools`

## References / raw notes

### Features

- 🎐 Nearly **20× smaller than an Electron package** (~5 MB).
- 🚀 Built on **Rust + Tauri**: faster startup and lower memory than JS frameworks.
- 📦 Battery-included — shortcut pass-through, immersive windows, drag-and-drop, style customisation, ad removal.
- 👻 Deliberately small in scope — replaces the "wrap a URL in Electron" pattern with a Tauri-based one.

### Getting started by audience

- **Beginners** — download a ready-made package from the [Releases](https://github.com/tw93/Pake/releases) page, or use the GitHub Actions online builder; no local toolchain required.
- **Developers** — install the CLI for one-command packaging with custom icons / window settings.
- **Advanced** — clone the repo and customise styles / behaviour locally.

### Popular pre-built packages

WeRead, Twitter/X, Grok, DeepSeek, ChatGPT, Gemini, YouTube Music, YouTube, LiZhi, ProgramMusic, Excalidraw, XiaoHongShu — all available for macOS, Windows, and Linux on the Releases page.

### CLI

```shell
# install
pnpm install -g pake-cli

# minimal
pake https://github.com --name GitHub

# full options
pake https://weekly.tw93.fun \
  --name Weekly \
  --icon https://cdn.tw93.fun/pake/weekly.icns \
  --width 1200 --height 800 \
  --hide-title-bar
```

### Building from source

Prerequisites: **Rust ≥ 1.85**, **Node ≥ 22**. See the [Tauri prerequisites guide](https://v2.tauri.app/start/prerequisites/) for OS-specific extras (Xcode CLT on macOS, MSVC + WebView2 on Windows, GTK/WebKit2GTK on Linux).

```shell
pnpm run dev      # local dev
pnpm run build    # produce platform bundle
```
