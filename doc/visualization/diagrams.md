---
title: "Diagrams: drawio + D2"
main_link: https://www.drawio.com/
keywords: [diagrams, drawio, d2lang, mermaid, plantuml, architecture-diagrams]
status: reviewed
review_date: 2026/05/03
---

# Diagrams: drawio + D2

**Main link:** <https://www.drawio.com/>

Repo (desktop app): <https://github.com/jgraph/drawio-desktop>

## Summary

Two complementary tools for the "I need a diagram" problem:

- **[drawio](https://www.drawio.com/)** (a.k.a. diagrams.net) — a free, GUI-driven, click-and-drag editor that runs in the browser, as a desktop app, or embedded in VS Code / Confluence / Jira. Files are saved as XML (or `.drawio`/`.drawio.svg`/`.drawio.png` so they round-trip), so they're git-friendly even though the editor is point-and-click. Works fully offline.
- **[D2](https://d2lang.com/)** — a modern declarative diagram language by [Terrastruct](https://terrastruct.com/), think "what Mermaid should have been". You write `a -> b: label`, you get a SVG/PNG. Great for architecture diagrams that live next to code in a repo.

## Insight

Use the right tool for the medium:

- **In a doc / wiki / slide deck**, drawio. The output looks polished out of the box and non-technical reviewers can edit it.
- **In a git repo / README / mdbook**, D2 (or Mermaid if your renderer supports it but not D2). Text-as-source means PRs can review diagram changes the same way they review code, and the rendered SVG is a build artifact rather than a binary blob.

The thing to avoid is reaching for [PlantUML](https://plantuml.com/) for new work — its DSL is older and less ergonomic than D2's, and you need a Java runtime to render it. The thing to also avoid is keeping diagrams *only* as Visio/OmniGraffle/Lucid files; both lock you into a vendor and break offline.

## Similar / related topics

- [Mermaid](https://mermaid.js.org/) — Markdown-native diagram language; supported by GitHub, GitLab, mdbook (via `mdbook-mermaid`), Obsidian. Less expressive than D2 but more widely rendered.
- [Excalidraw](https://excalidraw.com/) — hand-drawn-look whiteboard for sketches and quick brain dumps.
- [PlantUML](https://plantuml.com/) — older declarative DSL; avoid for new work.
- [[manim]] — when "diagram" really means "animation that explains a concept".
- [[rerun]] — when "diagram" really means "what is the robot/sensor seeing right now".

## Internal links

- [[manim]] — when you need an animation, not a static diagram
- [[rerun]] — multimodal time-aware visualisation (robotics)
- [[plotters]] — Rust crate for generating chart images programmatically

## Keywords

`#visualization` `#diagrams` `#drawio` `#d2lang` `#mermaid` `#architecture` `#docs-as-code`

## References / raw notes

- drawio web app: <https://app.diagrams.net/>
- drawio desktop releases: <https://github.com/jgraph/drawio-desktop/releases>
- D2 language: <https://d2lang.com/>
- D2 playground: <https://play.d2lang.com/>

drawio supports many output formats including `.drawio.svg` (an SVG with the editable XML embedded), which is a great pick when you want both git-diffable text *and* a directly viewable image.
