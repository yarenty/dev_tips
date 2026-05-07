---
title: QPML — Query Plan Markup Language
main_link: https://github.com/andygrove/qpml
keywords: [qpml, query-plan, yaml, dsl, andy-grove, documentation]
status: reviewed
---

# QPML — Query Plan Markup Language

**Main link:** <https://github.com/andygrove/qpml>

## Summary

QPML (Query Plan Markup Language) is a tiny YAML-based DSL by Andy Grove for **describing tree structures — query plans, expression trees, anything tree-shaped — for the purpose of producing diagrams and textual representations** in documentation, slide decks, and presentations. It is *not* a serialisation of any executable plan; think of it as the "Mermaid for query plans" — you write YAML, the tool emits ASCII / dot / PNG.

## Insight

Reach for QPML when you're authoring docs, blog posts, conference talks or course material that walks through query-plan transformations and you want consistent, version-controllable diagrams without hand-drawing each step. It is **not** a Substrait substitute (Substrait is the engine-portable IR for actually exchanging plans across systems — different problem). The audience is small: the project is a personal tool from the author of [[../data/datafusion/README|DataFusion]] / Ballista who needs to produce a lot of these diagrams. If you don't write that kind of content, you'll never need it. If you do, the output looks better than ASCII art and is much faster to iterate on than a hand-drawn diagram.

## Similar / related topics

- **Substrait** — engine-portable serialised query plan IR; the *executable* counterpart, not a doc tool.
- **Mermaid** / **Graphviz dot** / **D2** — generic tree/graph diagram DSLs you'd reach for if QPML's specific semantics aren't worth the buy-in.
- [[datafusion]] — the engine whose plans QPML was built to document.
- [[books|*How Query Engines Work*]] — also Andy Grove; QPML is a tool for writing more material like this.

## Internal links

<!-- reviewed -->
- [[README]]
- [[datafusion]]
- [[books]]
- [[../data/datafusion/README|DataFusion landing]]

## Keywords

`#qpml` `#query-plan` `#yaml` `#dsl` `#andy-grove` `#docs`

## References / raw notes

- Source: <https://github.com/andygrove/qpml>

> QPML is a YAML-based DSL for describing query plans, expression trees, or any other tree structure, for the purposes of producing diagrams and textual representations for use in documentation and presentations.

![QPML minimal example](https://github.com/andygrove/qpml/raw/main/examples/minimal.png)
