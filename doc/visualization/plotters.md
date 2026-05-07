---
title: "Plotters: pure-Rust drawing library for charts (+ a brief tour of GTK-rs)"
main_link: https://github.com/plotters-rs/plotters
keywords: [plotters, rust, charts, cairo, gtk-rs, wasm, gui]
status: reviewed
review_date: 2026/05/03
---

# Plotters: pure-Rust drawing library for charts (+ a brief tour of GTK-rs)

**Main link:** <https://github.com/plotters-rs/plotters>

Crates.io: <https://crates.io/crates/plotters> · Docs: <https://docs.rs/plotters>

## Summary

[Plotters](https://github.com/plotters-rs/plotters) is a pure-Rust drawing library for charts, plots, and figures. It abstracts over a back-end (the thing that actually paints pixels or vectors) so the same chart code can target `BitMapBackend` (PNG/JPEG), `SVGBackend`, `cairo` via [GTK-rs](https://gtk-rs.org/), [`piston_window`](https://github.com/PistonDevelopers/piston_window), or WebAssembly with `<canvas>`. The [Jupyter integration](https://plotters-rs.github.io/plotters-doc-data/evcxr-jupyter-integration.html) (via `evcxr`) makes it usable as a notebook plotting tool, and the [WASM demo](https://plotters-rs.github.io/wasm-demo/www/index.html) shows the same API rendering live in a browser.

## Insight

Plotters is what you reach for when:

- You're already in Rust and want to render a chart **without spawning Python**. Pulling in matplotlib via PyO3 is roughly 10× more painful than just using Plotters.
- You want **deterministic, file-output charts** for a CI report, a generated documentation page, or a benchmark dashboard. The `BitMapBackend` writes to disk and you're done.
- You want the **same chart code** to render to a desktop GUI (GTK), to a server-side PNG, and to a browser canvas (WASM). Few other libraries in any language give you that triple.

It is *not* the right tool when you need interactive charts with hover/zoom/brush — for that you want JS-land (Plotly, ECharts, [[svelvet]]) or [[visualization/grafana|grafana]] for dashboards. Plotters is fundamentally a *drawing* library, not a *visualization* framework.

The README links into a small constellation of GTK-rs demos (Czkawka, Solanum, Warp) which aren't really Plotters projects — they're examples of *Rust GUI apps* using the `gtk-rs` bindings, included here as a "what the GTK back-end ecosystem looks like" gesture. They've been kept in the references section below for the same reason.

## Similar / related topics

- [`charming`](https://github.com/yuankunzhang/charming) — Rust bindings to Apache ECharts; nicer for interactive HTML output.
- [`plotly` (Rust)](https://github.com/plotly/plotly.rs) — Rust bindings to plotly.js; SVG / HTML output.
- [`textplots`](https://github.com/loony-bean/textplots-rs) — when "plot" really means "ASCII chart in the terminal".
- [[visualization/grafana|grafana]] — for live dashboards rather than one-shot rendered charts.
- [`gtk-rs`](https://gtk-rs.org/) — the Rust GTK4 bindings; the back-end Plotters uses for desktop GUI rendering.

## Internal links

- [[visualization/grafana|grafana]] — when you want a live dashboard, not a one-shot PNG
- [[svelvet]] — JS-side alternative for interactive node-graph style visualisations
- [[diagrams]] — for static architecture diagrams (different problem)

## Keywords

`#visualization` `#plotters` `#rust` `#charts` `#cairo` `#gtk-rs` `#wasm` `#gui`

## References / raw notes

### Project pitch

> Plotters is a drawing library designed for rendering figures, plots, and charts, in pure Rust. Plotters supports various types of back-ends, including bitmap, vector graph, piston window, GTK/Cairo and WebAssembly.

Useful entry points:

- Source: <https://github.com/plotters-rs/plotters>
- Crate: <https://crates.io/crates/plotters>
- Jupyter (via `evcxr`) tutorial: <https://plotters-rs.github.io/plotters-doc-data/evcxr-jupyter-integration.html>
- Live WASM demo: <https://plotters-rs.github.io/wasm-demo/www/index.html>
- Demo apps: <https://github.com/plotters-rs/plotters-minifb-demo>, <https://github.com/plotters-rs/plotters-gtk-demo>

### Minimal example: a sine line chart to a PNG

```rust
use plotters::prelude::*;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let root = BitMapBackend::new("sine.png", (800, 480)).into_drawing_area();
    root.fill(&WHITE)?;

    let mut chart = ChartBuilder::on(&root)
        .caption("y = sin(x)", ("sans-serif", 30))
        .margin(20)
        .x_label_area_size(40)
        .y_label_area_size(40)
        .build_cartesian_2d(-6.3f32..6.3f32, -1.2f32..1.2f32)?;

    chart.configure_mesh().draw()?;
    chart.draw_series(LineSeries::new(
        (-630..=630).map(|x| x as f32 / 100.0).map(|x| (x, x.sin())),
        &BLUE,
    ))?;

    root.present()?;
    Ok(())
}
```

### GTK-rs detour (orphaned references from the original notes)

The original notes had a small grab-bag of Rust GUI projects built on the [`gtk-rs`](https://gtk-rs.org/) GTK4 bindings — these don't really belong in a Plotters article (they're not chart libraries) but are kept here as a starting point for the wider Rust+GTK4 ecosystem:

- **GTK4 Rust book**: <https://gtk-rs.org/gtk4-rs/stable/latest/book/>
- **`cairo-rs`** (Rust bindings to Cairo, used by both GTK-rs and Plotters' GTK back-end): <https://crates.io/crates/cairo-rs>
- **[Czkawka](https://github.com/qarmin/czkawka)** — fast duplicate-file finder; example of a non-trivial GTK4 Rust app.
- **[Warp (GNOME)](https://gitlab.gnome.org/World/warp)** — fast secure file transfer; same.
- **[Solanum](https://gitlab.gnome.org/World/Solanum)** — a Pomodoro app; GTK3-era and now mostly historical.
