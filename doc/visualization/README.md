---
title: Visualization
keywords: [visualization, charts, dashboards, diagrams, animation, fonts, pdf, templating]
status: reviewed
review_date: 2026/05/03
---

# Visualization

Notes on tools for *seeing* data and producing rendered output — charts, diagrams, dashboards, animations, PDFs, fonts. Spans Rust, Python, R, JavaScript, and the typesetting world.

## How the section is organised

The articles fall into a few buckets:

- **Static diagrams & animation** — [[diagrams]], [[manim]]
- **Charting libraries** — [[plotters]] (Rust), [[ggtech]] (R/ggplot2), [[svelvet]] (Svelte node-graphs)
- **Live dashboards** — [[visualization/grafana|grafana]] (the dashboard-canvas + Mimir framing; see [[observability/grafana|grafana]] for the operator's view)
- **Multimodal / time-aware viewers** — [[rerun]] (robotics, embodied AI)
- **Inspiration & taste** — [[portfolios]], [[pudding]], [[dataviz_resources]]
- **Typography** — [[fonts]] (programming fonts)
- **Documents** — see the `pdf/` and `web_templating/` subsections

## Articles

- [[diagrams]] — drawio + D2: GUI editor and declarative DSL for diagrams.
- [[manim]] — programmatic animation engine for math explainer videos.
- [[plotters]] — pure-Rust drawing library for charts; bitmap / SVG / GTK / WASM back-ends.
- [[ggtech]] — ggplot2 themes mimicking tech-company chart styles.
- [[svelvet]] — Svelte component library for node-graph editors.
- [[visualization/grafana|grafana]] — dashboard canvas + Mimir for long-term metrics.
- [[rerun]] — time-aware multimodal viewer for robotics & embodied AI.
- [[portfolios]] — practitioners' portfolios worth studying for chart-design taste.
- [[pudding]] — *The Pudding* — long-form data-essay journalism + open datasets.
- [[dataviz_resources]] — books, courses, references for the craft itself.
- [[fonts]] — programming fonts (Iosevka, Fira Code, Victor Mono, …).

## Subsections

- [PDF documents](pdf/README.md) — generating PDFs: [[typst]], [[react]], [[mdmath_symbols]].
- [Web templating](web_templating/README.md) — [[liquid]] (and how it compares to Jinja/Handlebars/Tera).

## Cross-section see-also

- [[observability/grafana|grafana]] — operator's-view article on Grafana under `observability/`.
- [[prometheus]] — the canonical metrics datasource for Grafana.
- [[groot]] — example of a system whose internal state Rerun is good at visualising.
- [Published](../published/README.md) — when "visualization" is really a "ship a polished doc" problem.

## Keywords

`#visualization` `#charts` `#dashboards` `#diagrams` `#animation` `#fonts` `#pdf` `#templating`
