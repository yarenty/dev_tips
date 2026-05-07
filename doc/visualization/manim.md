---
title: "Manim: programmatic animation engine for math videos"
main_link: https://www.manim.community/
keywords: [manim, animation, math, 3blue1brown, python, explainer-videos]
status: reviewed
review_date: 2026/05/03
---

# Manim: programmatic animation engine for math videos

**Main link:** <https://www.manim.community/>

Repo: <https://github.com/ManimCommunity/manim>

## Summary

[Manim](https://www.manim.community/) ("Mathematical Animation Engine") is a Python library for generating precisely-animated math diagrams as MP4/GIF/PNG. You write Python that subclasses `Scene` and emits `Create(...)`, `Transform(...)`, `Write(...)` etc. for each step; Manim renders it via Cairo/OpenGL. It originated as the private tooling [3Blue1Brown / Grant Sanderson](https://www.3blue1brown.com/) used for his YouTube channel; the **community fork** (`manimce`) at <https://github.com/ManimCommunity/manim> is the actively-maintained version everyone now uses, while the original `manim` repo at <https://github.com/3b1b/manim> still works but tracks Grant's personal needs.

## Insight

Reach for Manim when:

- You need **mathematical precision** — exact LaTeX rendering, exact coordinates, exact transforms between equations. No other animation tool comes close for math.
- You're producing **explainer videos** where the steps need to land in a specific order with specific timing, and you want to re-render the whole scene any time you tweak one number.
- You want **diff-able animations** in git. Because the source is Python, edits and reviews happen on text, not on a 1.5 GB After Effects project file.

Don't reach for Manim when:

- You want a UI animation tool — use [Lottie](https://airbnb.io/lottie/) / [Rive](https://rive.app/).
- You want hand-drawn-look sketches — [Excalidraw](https://excalidraw.com/) is faster.
- You just need a static diagram — [[diagrams]] (drawio + D2) is one or two orders of magnitude less effort.

The two-step process is always the same: write `class MyScene(Scene): def construct(self): ...`, then run `manim -pql my_file.py MyScene` (the `-pql` flag = preview, low quality, fast iteration). When you ship, swap to `-qh` (high quality).

## Similar / related topics

- [3Blue1Brown YouTube](https://www.youtube.com/@3blue1brown) — the canonical example of what Manim is *for*. If you've never seen one of these videos, watch one before deciding whether to invest the time.
- [Motion Canvas](https://motioncanvas.io/) — TypeScript alternative; web-native, browser preview, broader than just math.
- [Reanimate](https://reanimate.github.io/) — Haskell equivalent.
- [[diagrams]] — when a static diagram is enough.
- [[rerun]] — when "animation" really means "live multimodal data over time".

## Internal links

- [[diagrams]] — static-diagram alternative
- [[rerun]] — for live data, not for explainers

## Keywords

`#visualization` `#manim` `#animation` `#math` `#3blue1brown` `#python` `#explainer-videos`

## References / raw notes

- Community docs: <https://docs.manim.community/>
- Original (3b1b) repo: <https://github.com/3b1b/manim>
- Tutorial collection: <https://docs.manim.community/en/stable/tutorials_guides.html>

The community fork ships as the `manim` PyPI package. **Do not** confuse with `manimlib` (the original) — they have diverged in subtle ways (slightly different class hierarchy, different default render backend) and copy-pasted code from one will often not run on the other.
