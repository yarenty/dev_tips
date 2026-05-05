---
title: "ggtech: ggplot2 themes for tech-company logos"
main_link: https://github.com/ricardo-bion/ggtech
keywords: [ggtech, ggplot2, r, charts, themes, branded-charts]
status: reviewed
---

# ggtech: ggplot2 themes for tech-company logos

**Main link:** <https://github.com/ricardo-bion/ggtech>

## Summary

[`ggtech`](https://github.com/ricardo-bion/ggtech) is a small R package by [Ricardo Bion](https://github.com/ricardo-bion) (ex-Airbnb data scientist) that provides [ggplot2](https://ggplot2.tidyverse.org/) themes and palettes mimicking the in-house chart styles of well-known tech companies — Airbnb, Etsy, Facebook, Google, Twitter. You add `+ theme_tech(theme = "airbnb")` and your scatter plot suddenly looks like it came out of one of their data-team blog posts.

## Insight

Mostly a curiosity / blog-post-aesthetic generator now, but worth knowing about for two reasons:

1. As a **reference** for what good "branded chart" defaults look like — sensible greys, one accent colour, restrained gridlines, generous margins, decent fonts. These are good rules even outside ggplot2.
2. As a reminder that **ggplot2's `theme()` system is just composable styling** — once you've read one of these branded themes, writing your own is straightforward.

For most actual analysis the default `theme_minimal()` or [hrbrthemes](https://github.com/hrbrmstr/hrbrthemes) (Bob Rudis's typography-focused themes) are the better picks.

## Similar / related topics

- [hrbrthemes](https://github.com/hrbrmstr/hrbrthemes) — Bob Rudis's themes; the more actively maintained "good defaults" alternative.
- [ggthemes](https://github.com/jrnold/ggthemes) — themes mimicking The Economist, FiveThirtyEight, Tufte, etc.
- [ggplot2](https://ggplot2.tidyverse.org/) — the underlying grammar-of-graphics library.
- [seaborn](https://seaborn.pydata.org/) — the closest "good defaults out of the box" equivalent for Python.
- [[plotters]] — Rust analogue if you want to render charts from a Rust pipeline.

## Internal links

<!-- reviewed -->

- [[plotters]] — Rust chart library
- [[dataviz_resources]] — books / courses on what makes a chart good in the first place

## Keywords

`#visualization` `#ggtech` `#ggplot2` `#r` `#charts` `#themes`

## References / raw notes

Repo: <https://github.com/ricardo-bion/ggtech> · Author's blog (where the original announcement and chart examples lived): <https://medium.com/@bion>.

The package ships:

- `theme_tech(theme = ...)` — `airbnb`, `etsy`, `facebook`, `google`, `twitter`, `X23andme`.
- `scale_color_tech` / `scale_fill_tech` — matching colour palettes.
- A small set of bundled font files; you'll need to install them on your system for the themes to render correctly.
