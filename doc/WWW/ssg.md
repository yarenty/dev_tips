---
title: Static Site Generators (Zola, Tera)
main_link: https://www.getzola.org/
keywords: [ssg, zola, tera, static-site-generator, rust, sass, jinja]
status: reviewed
---

# Static Site Generators (Zola, Tera)

**Main link:** <https://www.getzola.org/>

## Summary

Notes on **Zola** — a fast, single-binary, dependency-free static-site generator written in Rust — and its templating engine **Tera** (Jinja2-style, also Rust). Zola compiles a tree of Markdown into HTML/CSS/JS in under a second, with built-in Sass compilation, syntax highlighting, table of contents, taxonomies (tags/categories), shortcodes, and internal links — no Node, no Ruby, no plugin chain.

## Insight

The right choice for **personal blogs, project documentation, knowledge bases, and conference / workshop landing pages** that don't need a server. Single-binary install (`brew install zola` or one curl), no `node_modules` to babysit, and the build is fast enough that you can use `zola serve` as a live-preview during writing. Tera as the templating engine is comfortable if you've used Jinja2 or Django templates.

When **not** to reach for Zola:
- You need real interactivity / forms / auth → see [[loco_rust_on_rails|Loco]] or any full web framework.
- Your team is JS-native and wants component-based authoring → Astro, Next.js, SvelteKit, or Hugo with theme components.
- You need plugin ecosystem breadth → Hugo or 11ty have far more themes and integrations.

In the Rust ecosystem, Zola's main competition is `mdBook` (which is what *this* vault publishes with) — Zola is more general-purpose, mdBook is laser-focused on book/handbook output.

## Similar / related topics

- **mdBook** — sibling Rust SSG focused on handbook/book output; what this very vault uses.
- **Hugo** — Go-based SSG; the de-facto incumbent for blogs and personal sites.
- **Eleventy (11ty)** — Node-based SSG; very flexible templating.
- **Astro / Next.js (export)** — when you want React-style components on a static output.
- [[loco_rust_on_rails|Loco]] — the *opposite* of an SSG; full server-rendered framework.

## Internal links

<!-- reviewed -->

- [[loco_rust_on_rails]] — Server-rendered counterpart; Zola is the "static" half of the same web-dev decision tree.
- [[fluent_templates]] — i18n layer that pairs naturally with Tera (Zola's template engine).

## Keywords

`#ssg` `#zola` `#tera` `#static-site-generator` `#rust` `#sass` `#jinja`

## References / raw notes

### Zola

- Project site: <https://www.getzola.org/>
- Repo: <https://github.com/getzola/zola>
- Getting-started overview: <https://www.getzola.org/documentation/getting-started/overview/>
- Themes: <https://www.getzola.org/themes/>

Project pitch (from the home page):

> **No dependencies** — single executable with Sass compilation, syntax highlighting, table of contents.
> **Blazing fast** — average site generated in <1 s including Sass and highlighting.
> **Scalable** — pure static files; trivial to host.
> **Easy to use** — intuitive CLI and template engine.
> **Flexible** — fits blogs, knowledge bases, landing pages.
> **Augmented Markdown** — shortcodes and internal links built in.

### Tera

- Project site: <https://keats.github.io/tera/>
- Repo: <https://github.com/Keats/tera>

Templating engine that powers Zola (and is usable standalone). Jinja2 / Django templates / Liquid / Twig if you've used any of those.

Pitch:

> Easy to use. Performant — template compilation and rendering measured in nanoseconds. Familiar — Jinja2 / Django / Liquid / Twig syntax.

### Quick start with Zola

```shell
brew install zola             # macOS / Linuxbrew
# or:  cargo install zola

zola init my-site
cd my-site
# Add content/ pages, templates/ files, sass/ partials, themes/ if any.

zola serve                    # live-reload dev server on http://127.0.0.1:1111
zola build                    # produces public/ for deployment
```

### Note on Dioxus

The original raw notes mentioned **Dioxus** as something to check; that's a *different* product (a Rust UI framework, not an SSG). See `programming/rust/gui/dioxus.md` for the actual notes.
