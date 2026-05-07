---
title: Pake
main_link: https://github.com/tw93/Pake
keywords: [pake, rust, tauri, web-to-app, webview, desktop, packaging]
status: reviewed
---

# Pake

**Main link:** <https://github.com/tw93/Pake>

## Summary

Pake is a small Rust + Tauri tool that wraps any web URL into a thin native desktop app for macOS, Windows, or Linux. The output bundle is roughly 5 MB — about 20× smaller than the equivalent Electron build — because it uses the OS-native webview (WKWebView / WebView2 / WebKitGTK) instead of bundling Chromium. Maintained by `tw93`, Pake exposes a `pake-cli` (npm-installable) plus a small set of pre-packaged "popular packages" you can install directly.

> **Filename ambiguity**: this article (Tauri-Pake, the wrapper tool) lives at `programming/rust/gui/pake.md`. There is a *separate* `tools/security/pake.md` about the **Pake** cryptographic protocol (Password-Authenticated Key Exchange). Always link this article as `[[programming/rust/gui/pake|pake]]` from outside `gui/`.

## Insight

Pake is the right pick when you want to ship a webapp as a desktop bundle and you don't need any deep OS integration — *if* you do, jump straight to writing your own [[programming/rust/gui/tauri|tauri]] app, because Pake is essentially a curated default Tauri config plus a CLI. Compared to the JS-only competition: Electron is fatter (often 80–150 MB) but offers Node APIs in the renderer and consistent Chromium behaviour; [Wails](https://wails.io/) (Go) is the closest Pake-shaped alternative outside the Rust world; [Neutralino](https://neutralino.js.org/) is even smaller but uses the OS webview without a strong CLI ergonomics layer. Two real-world gotchas: (1) the OS webview means each platform behaves slightly differently (right-click image-save is broken on macOS WebKit), and (2) PWA install is genuinely "good enough" for a lot of cases — try that before reaching for Pake.

```bash
# Install
npm install -g pake-cli

# Wrap any URL
pake https://weekly.tw93.fun --name Weekly --hide-title-bar
```

## Similar / related topics

- [[programming/rust/gui/tauri|tauri]] — the framework underneath; reach for it when Pake is too restricted.
- Electron — the gold-standard JS-everything alternative; much bigger bundles.
- Wails (Go) — same shape, Go backend instead of Rust.
- Neutralino — even thinner cross-platform webview shell.
- PWA / "Install as App" — try first; zero install for you, browser-managed for users.

## Internal links

- [[programming/rust/gui/tauri|tauri]]
- [[dioxus]]
- [[../README]]
- [[tools/security/pake|pake (crypto, different thing)]]

## Keywords

`#pake` `#rust` `#tauri` `#web-to-app` `#webview` `#desktop` `#packaging`

## References / raw notes

- Repo: <https://github.com/tw93/Pake>
- CLI docs: see `bin/README.md` in the repo
- Online compilation via GitHub Actions: <https://github.com/tw93/Pake/wiki/Online-Compilation-(used-by-ordinary-users)>
- Advanced usage: <https://github.com/tw93/Pake/wiki/Advanced-Usage-of-Pake>

### Features

- ~20× smaller than an Electron package (around 5 MB)
- Uses Tauri / WRY under the hood — lighter and faster than JS-based shells
- Battery-included: shortcut pass-through, immersive windows, minimal customisation
- Replaces older bundling approaches (PWA is often good enough; this is the next step up)

### Quick recipes

```bash
# Install
npm install -g pake-cli

# Pack with options
pake url [OPTIONS]...
pake https://weekly.tw93.fun --name Weekly --hide-title-bar
```

If you don't want a local toolchain, the project provides a [GitHub Actions online compilation](https://github.com/tw93/Pake/wiki/Online-Compilation-(used-by-ordinary-users)) flow.

### Local development

Requires Rust ≥ 1.63 and Node ≥ 16. See [Tauri prerequisites](https://tauri.app/v1/guides/getting-started/prerequisites).

```sh
# Install dependencies
npm i

# Local dev (right-click to open debug mode)
npm run dev

# Pack the app
npm run build
```

### Customisation

- Modify `pake.json` and `tauri.config.json` under `src-tauri/` for `url`, `productName`, `domain`, icon, identifier, window properties (`width`, `height`, `fullscreen`, `resizable`).
- For macOS immersive header, set `hideTitleBar: true` and add `padding-top` to the `Header` element.
- Style rewriting, ad removal, JS injection, container messaging, and custom shortcuts: see [Advanced Usage](https://github.com/tw93/Pake/wiki/Advanced-Usage-of-Pake).

### FAQ

- Right-click → "Save Image" doesn't work on macOS — limitation of the system webview, not Pake.

### Codebase structure

See the [code structure description](https://github.com/tw93/Pake/wiki/Description-of-Pake's-code-structure) before contributing.
