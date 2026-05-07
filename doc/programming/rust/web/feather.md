---
title: Feather
main_link: https://github.com/BersisSe/feather
keywords: [feather, rust, web-framework, microframework, express]
status: reviewed
---

# Feather

**Main link:** <https://github.com/BersisSe/feather>

## Summary

Feather is a small, Express.js-inspired Rust web microframework — middleware-first, macro-light, with a `Context`-API for state instead of Axum-style extractors. It's a hobbyist/early-stage project (single primary author, BersisSe), aimed at developer ergonomics and a low surface area rather than at competing with Axum or actix-web on production features. Bundles a CLI for scaffolding.

## Insight

Reach for Feather only if you specifically want the Express.js mental model in Rust — every route handler and cross-cutting concern is "just middleware" — and you're comfortable with a small, young project that may not see long-term maintenance. For anything serious, the Rust ecosystem has consolidated around `[[axum]]` (similar middleware composability via `tower`), `[[rocket]]` (more opinionated), or actix-web (highest perf). Don't confuse this Feather with Apache Arrow's `feather` file format, the Minecraft-server `feather`, or the Python `feather-format` library — naming collisions are unfortunate.

## Similar / related topics

- **axum** — the production-grade analogue with a richer ecosystem.
- **rocket** — also macro-driven and DX-first, but more conventions.
- **tide** — async-std's small framework; effectively unmaintained.
- **Express.js** (Node) — the conceptual ancestor.
- **poem** — middleware + extractors; small and pleasant.

## Internal links
- [[axum]] — the obvious upgrade path
- [[rocket]] — the other DX-first option
- [[README|web README]] — sibling frameworks at a glance

## Keywords

`#feather` `#web` `#rust` `#microframework` `#middleware` `#express`

## References / raw notes

- Repo: <https://github.com/BersisSe/feather>

Project pitch (from the README):

> Feather is a lightweight, DX-first web framework for Rust — inspired by the simplicity of Express.js, but designed for Rust's performance and safety.

Why Feather (project's own framing):

- **Middleware-first architecture** — every route handler, auth, logging is composable middleware.
- **Easy state management via `Context`** — recent Context API, no extractors / no macros.
- **DX-first** — minimal, ergonomic, readable API.
- **Modular and extensible** — opt-in features only.
- **Feather-CLI** — scaffolding tool for creating APIs / web servers.
