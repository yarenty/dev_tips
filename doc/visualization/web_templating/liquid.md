---
title: "Liquid: Shopify's safe template language"
main_link: https://shopify.github.io/liquid/
keywords: [liquid, shopify, templating, jekyll, safe-eval, ruby, rust]
status: reviewed
---

# Liquid: Shopify's safe template language

**Main link:** <https://shopify.github.io/liquid/>

Reference repo (Ruby): <https://github.com/Shopify/liquid> · Rust port: <https://lib.rs/crates/liquid>

## Summary

[Liquid](https://shopify.github.io/liquid/) is a templating language created by Shopify in 2006 specifically so merchants could customise their storefronts *without* being able to execute arbitrary code on Shopify's servers. The whole language is intentionally restricted: variables are dot-paths (`product.title`), control flow is `{% if %}` / `{% for %}`, and "filters" (`| upcase`, `| date: "%Y-%m-%d"`, `| money`) are the only place data can be transformed. There's no way to call into the host application unless the host explicitly exposes it.

That same safety property made it the natural pick for [Jekyll](https://jekyllrb.com/) (Markdown → static site, the engine behind GitHub Pages) and a steady stream of "user-supplies-templates" tools since.

## Summary of implementations worth knowing

- [`liquid` (Ruby)](https://github.com/Shopify/liquid) — the canonical Shopify implementation; what Jekyll uses.
- [`liquid` (Rust)](https://lib.rs/crates/liquid) — feature-parity Rust port; what to use from a Rust web stack.
- [`liquidjs`](https://liquidjs.com/) — Node port; what to use if you're rendering Liquid in a JS toolchain (Eleventy, etc.).
- [`python-liquid`](https://github.com/jg-rp/liquid) — Python port; useful if your back-end is Django/Flask/FastAPI.

## Insight

Reach for Liquid (instead of [Jinja2](https://jinja.palletsprojects.com/), [Handlebars](https://handlebarsjs.com/), or [Tera](https://keats.github.io/tera/)) when:

- You're letting **end users author templates**. This is the original Shopify use-case. Jinja2 and Handlebars both have escape hatches into the host language (e.g. Jinja's `{{ ''.__class__.__mro__ }}` chain) that have caused real RCEs; Liquid was designed not to.
- You're working in the **Jekyll / Eleventy / Shopify ecosystem** and would benefit from sharing template syntax with everything else in the toolchain.

Reach for one of the alternatives when:

- You're writing the templates yourself and want maximum expressiveness — [Jinja2](https://jinja.palletsprojects.com/) (Python) or [Tera](https://keats.github.io/tera/) (Rust, Jinja-shaped) are more powerful.
- You're in a JS app that already uses [Handlebars](https://handlebarsjs.com/) or [EJS](https://ejs.co/) or [Mustache](https://mustache.github.io/).
- You want the type-checking and syntax familiarity of [JSX](https://react.dev/learn/writing-markup-with-jsx) — though that's not really a templating language anymore.

## Similar / related topics

- [Jinja2](https://jinja.palletsprojects.com/) — Python's dominant template language; more expressive but riskier for untrusted input.
- [Tera](https://keats.github.io/tera/) — Jinja-shaped Rust template engine (used in Zola).
- [Handlebars](https://handlebarsjs.com/) — JavaScript-side; logic-less by design.
- [Mustache](https://mustache.github.io/) — even more logic-less; ports in every language.
- [Jekyll](https://jekyllrb.com/) / [Eleventy](https://www.11ty.dev/) — static-site generators that consume Liquid templates.

## Internal links

- [[diagrams]] — for when "templating" is really an architecture-diagram problem
- [[plotters]] — for charts that should land in templated reports

## Keywords

`#visualization` `#web-templating` `#liquid` `#shopify` `#templating` `#jekyll` `#safe-eval` `#ruby` `#rust`

## References / raw notes

### Tiny Liquid example

```liquid
{% if product.available %}
  <h1>{{ product.title | upcase }}</h1>
  <p>{{ product.price | money }}</p>
  {% for variant in product.variants %}
    <li>{{ variant.title }}</li>
  {% endfor %}
{% else %}
  <p>Sold out — sorry!</p>
{% endif %}
```

The two delimiters, in case you've never seen them:

- `{{ ... }}` for **output** (interpolated values, with optional `| filter` chain).
- `{% ... %}` for **control flow** (no output: `if`, `for`, `assign`, `capture`, `unless`, `case`, ...).

### Useful entry points

- Language docs: <https://shopify.github.io/liquid/>
- Ruby reference impl: <https://github.com/Shopify/liquid>
- Rust port (crates.io): <https://lib.rs/crates/liquid>
- LiquidJS (Node): <https://liquidjs.com/>
- Liquid is at the heart of Jekyll: <https://jekyllrb.com/docs/liquid/>
