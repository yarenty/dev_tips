---
title: egui
main_link: https://www.egui.rs/
keywords: [egui, rust, gui, immediate-mode, eframe, wasm]
status: reviewed
review_date: 2026/05/03
---

# egui

**Main link:** <https://www.egui.rs/>

## Summary

`egui` is a portable immediate-mode GUI library for Rust by Emil Ernerfelt (`emilk`). The companion crate `eframe` wraps it into a runnable application that targets native (winit + wgpu/glow) and the web (WASM + WebGL/WebGPU) from the same source. Because all egui needs is the ability to draw textured triangles, it slots cleanly into game engines and custom renderers as a debug overlay or tool UI.

## Insight

egui's killer use case is *internal tools, dev panels, debug overlays, and small utilities* — anywhere "I need a working UI by tomorrow" beats "I need pixel-perfect native widgets". The immediate-mode model (the entire UI is rebuilt every frame from your state) is the same idea as Dear ImGui in C++ (Ocornut) but with idiomatic Rust ergonomics; if you grew up with retained-mode toolkits like Qt/GTK/WinUI, the lack of a persistent widget tree feels weird at first and freeing soon after. The trade-off is animation, accessibility (screen readers), and complex text layout (RTL, IME) are weaker than retained-mode frameworks like [[slint]] or native toolkits — egui has improved on all three but isn't where you go to ship a polished consumer app. For "GUI as visual REPL", web demo at <https://www.egui.rs/#demo> is the fastest way to see what it can do.

## Similar / related topics

- Dear ImGui — the C++ original of immediate-mode GUI; egui is the spiritual Rust port.
- [[slint]] — declarative, retained-mode alternative; better for polished consumer UI.
- [[../gui/dioxus|dioxus]] — React-shaped retained-mode if you want web tech alongside native.
- [[programming/rust/gui/tauri|tauri]] — web shell alternative when web tooling is a feature, not a cost.
- `egui_extras`, `egui_plot`, `egui_dock` — the active companion crate ecosystem.

## Internal links

- [[slint]]
- [[dioxus]]
- [[programming/rust/gui/tauri|tauri]]
- [[ui]]
- [[../README]]

## Keywords

`#egui` `#rust` `#gui` `#immediate-mode` `#eframe` `#wasm`

## References / raw notes

- Site / live demo: <https://www.egui.rs/#demo>
- Repo: <https://github.com/emilk/egui>
- Crate: <https://crates.io/crates/egui>
- Docs: <https://docs.rs/egui>
- Extras: <https://crates.io/crates/egui_extras>

egui is a simple, fast, and highly portable immediate-mode GUI library for Rust. It runs on the web, natively, and inside game engines. egui can be used anywhere you can draw textured triangles, which means you can easily integrate it into your game engine of choice.
