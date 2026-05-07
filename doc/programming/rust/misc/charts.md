---
title: Rust charting / plotting crates
main_link: https://github.com/plotters-rs/plotters
keywords: [charts, rust, plotting, visualization, plotters, plotly, charming]
status: reviewed
review_date: 2026/05/03
---

# Rust charting / plotting crates

**Main link:** <https://github.com/plotters-rs/plotters>

## Summary

This article is a small landscape map of charting and plotting in Rust. The historically-pinned link here was [`rustplotlib`](https://github.com/askanium/rustplotlib) (a small D3-inspired bar/line/area/scatter library), but it has not been actively maintained for several years. The modern Rust plotting picture in 2024–2025 is centred on a handful of options: `plotters` for backend-agnostic 2D drawing, `charming` (Apache ECharts wrapper) for rich interactive HTML output, `plotly` (rs) for Plotly.js dashboards, and `egui_plot` for live charts inside `egui` GUIs.

## Insight

Decision shortcut:

| You want… | Reach for |
|---|---|
| Programmatic 2D drawing → PNG/SVG/bitmap, embeddable in GUI/WASM | [[../../../visualization/plotters\|plotters]] |
| Interactive HTML charts (zoom/pan/hover, ECharts feature parity) | [`charming`](https://github.com/charming-rs/charming) |
| Plotly.js dashboards from Rust | [`plotly`](https://github.com/plotly/plotly.rs) |
| Live charts inside an `egui` GUI | [`egui_plot`](https://crates.io/crates/egui_plot) |
| Tiny terminal-only sparklines / bar charts | [`textplots`](https://crates.io/crates/textplots), [`ratatui`'s charts](https://docs.rs/ratatui/) |
| ggplot2-style grammar of graphics | [`charts-rs`](https://github.com/vicanso/charts-rs) (closer to ECharts), no exact ggplot port yet |

A few notes:

- **`rustplotlib`** (this article's original main link) is fine for one-off static D3-style charts but unmaintained; use `plotters` if you want the same role with active development.
- **For dataframe-style "give me a plot of this column"**, the closest Rust experience is `polars` → export to Arrow → render with `plotly` / `charming`. There is no first-class `df.plot()` equivalent yet.
- **For scientific/technical plots in PDF**, the practical workflow is still "render with `plotters` to SVG, embed in `typst`/`latex`."

## Similar / related topics

- [[../../../visualization/plotters|plotters]] — the de-facto pure-Rust 2D plotting crate.
- [[../../../visualization/README|visualization]] — vault-wide visualization section (also covers diagrams, fonts, manim, rerun).
- [`charming`](https://github.com/charming-rs/charming) — Apache ECharts wrapper.
- [`plotly`](https://github.com/plotly/plotly.rs) — Plotly.js bindings.
- [`egui_plot`](https://crates.io/crates/egui_plot) — for in-GUI live charts.

## Internal links

- [[../../../visualization/plotters|plotters]] — canonical Rust plotting crate (covered in P5.I).
- [[../../../visualization/README|visualization]] — vault-wide viz section.
- [[../gui/egui|egui]] — for embedding plots in immediate-mode GUIs.

## Keywords

`#charts` `#rust` `#plotting` `#visualization` `#plotters` `#plotly` `#charming`

## References / raw notes

- Original main link (now slow-moving): <https://github.com/askanium/rustplotlib>
- Modern de-facto crate: <https://github.com/plotters-rs/plotters>
- Apache ECharts wrapper: <https://github.com/charming-rs/charming>
- Plotly.js Rust bindings: <https://github.com/plotly/plotly.rs>
- `rustplotlib` originally supported: vertical bar, vertical stacked bar, horizontal bar, horizontal stacked bar, scatter, line, area; histograms / box plots / composite charts were marked TBD.
