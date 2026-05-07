---
title: Rust GUI landscape
main_link: https://www.areweguiyet.com/
keywords: [ui, rust, gui, landscape, floem, eww, seelen, 7guis]
status: reviewed
---

# Rust GUI landscape

**Main link:** <https://www.areweguiyet.com/>

## Summary

Catch-all for the broader Rust UI landscape — the "Are We GUI Yet?" picture and the smaller projects that don't yet warrant their own article: **floem** (lapce's fine-grained-reactive native UI library, Xilem/Leptos/rui-inspired), **EWW** (Elkowars Wacky Widgets — Rust + YuckLisp config for custom desktop widgets on any window manager), and **Seelen UI** (a customisable desktop environment for Windows). The canonical exercise for comparing toolkits across languages is Eugen Kiss's [7GUIs](https://eugenkiss.github.io/7guis/tasks) task list.

## Insight

If you're picking a Rust GUI toolkit for a new project, the practical short list as of 2024–2025 is: [[egui]] (immediate-mode, fastest to ship), [[slint]] (declarative, embedded sweet spot), [[ised|iced]] (Elm-architecture, type-safe), [[programming/rust/gui/tauri|tauri]] (web-tech shell), [[dioxus]] (React-shaped, cross-platform). The newer **floem** is interesting if you want signals-style fine-grained reactivity *natively* — it's the visible UI library inside the Lapce editor, so it's exercised in production. **EWW** and **Seelen UI** are end-user customisation tools, not frameworks you build *on*. For honest comparison-as-you-shop, build the same 7GUIs task (e.g. the Temperature Converter or Cells) in two candidates and see which one feels right — the canonical "GUI smoke test" Kiss originally proposed in 2014, still the best dual-test for a toolkit's ergonomics.

## Similar / related topics

- "Are We GUI Yet?" (areweguiyet.com) — community-maintained landscape index.
- 7GUIs — Eugen Kiss's seven canonical tasks for comparing toolkits.
- floem — Lapce's native fine-grained-reactive UI library; declarative + signals.
- EWW (Elkowars Wacky Widgets) — Rust+Yuck for custom WM widgets.
- Seelen UI — customisable Windows desktop shell built on Tauri.

## Internal links

<!-- reviewed -->
- [[egui]]
- [[slint]]
- [[ised|iced]]
- [[programming/rust/gui/tauri|tauri]]
- [[dioxus]]
- [[swww]]
- [[../README]]

## Keywords

`#rust-gui` `#landscape` `#floem` `#eww` `#7guis` `#seelen-ui`

## References / raw notes

- Are We GUI Yet?: <https://www.areweguiyet.com/>
- 7GUIs tasks: <https://eugenkiss.github.io/7guis/tasks>

### floem (lapce)

- Repo: <https://github.com/lapce/floem>
- Editor example: <https://github.com/lapce/floem/blob/main/examples/editor/src/main.rs>

A native Rust UI library with fine-grained reactivity. Inspired by Xilem, Leptos, and rui. Cross-platform (Windows / macOS / Linux) with `wgpu` rendering and a `tiny-skia` CPU fallback. Reactive signals modelled on `leptos_reactive`; Flexbox + Grid layout via Taffy; a styling/theming system with class support and third-party themes; CSS-style transitions and full keyframe animations with spring easing; built-in element inspector for debugging layout. The view tree is constructed once, so accidental rebuild bottlenecks are hard to introduce.

### EWW (Elkowars Wacky Widgets)

- Repo: <https://github.com/elkowar/eww>

Standalone widget system written in Rust that lets you implement custom desktop widgets in any window manager, configured in the Yuck Lisp-shaped DSL. Documentation in the repo; a beginner-friendly intro by Dharmx is also linked from there.

### Seelen UI

- Repo: <https://github.com/eythaann/Seelen-UI>

Fully customisable desktop environment for Windows. Built on Tauri. Available in 70+ languages.

### See also

- See [[dioxus]] / Freya for the React-shaped path.
- The auto-split sibling [[swww]] is the Wayland animated wallpaper daemon.
