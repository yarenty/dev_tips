---
title: Map of GitHub (anvaka)
main_link: https://anvaka.github.io/map-of-github
keywords: [github-map, anvaka, visualization, discovery, repos]
status: reviewed
---

# Map of GitHub (anvaka)

**Main link:** <https://anvaka.github.io/map-of-github>

## Summary

Andrei Kashcha's "Map of GitHub" treats every repository on GitHub as a point in 2-D space, clustered by which other repositories they share stargazers with. The resulting "map" looks like an actual atlas: continents of `web-development`, `machine-learning`, `game-dev`, peninsulas of niche tooling, islands of weird hobby projects. It's a discovery tool — pan around to find clusters of projects related to one you already know.

## Insight

Reach for it when you want to *discover adjacent crates* in an area you only know one or two entries in: find the `rust` cluster, zoom in, and you'll see the gravitational neighbours of `tokio`, `serde`, `clap`. Repository popularity is encoded as font size, so you can quickly distinguish established projects from drive-by experiments. Caveats: the dataset is a snapshot (refreshed periodically, not live), and the layout is built from stargazer overlap so projects with cult followings can sit far from objectively-related ones. A useful complement to crates.io's category browser when you don't yet know the right keyword to search for.

## Similar / related topics

- [Map of HN](https://anvaka.github.io/map-of-hn/) — same author, applied to Hacker News stories.
- [crates.io categories](https://crates.io/categories) — official taxonomic browse for Rust packages.
- [lib.rs](https://lib.rs/) — alternative crates.io front-end with hand-curated category trees.
- [awesome-rust](https://github.com/rust-unofficial/awesome-rust) — hand-curated; see [[_must_have]].
- [GitHub Topics](https://github.com/topics) — official tag-based discovery.

## Internal links
<!-- reviewed -->
- [[_must_have]]
- [[_to_learn]]
- [[README]]

## Keywords

`#github` `#map` `#anvaka` `#visualization` `#discovery`

## References / raw notes

- Live map: <https://anvaka.github.io/map-of-github>
- Source / methodology: <https://github.com/anvaka/map-of-github>
- Author: Andrei Kashcha (anvaka) — also built `pm.anvaka.com` (npm dependency map) and `vivagraphjs`.
