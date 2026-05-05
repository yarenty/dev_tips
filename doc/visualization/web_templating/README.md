---
title: Web templating
keywords: [web-templating, liquid, jinja, handlebars, mustache, tera]
status: reviewed
---

# Web templating

Templating languages for generating HTML (and increasingly anything else text-shaped). The choice almost always comes down to *who writes the templates*:

- **End users / untrusted input** → [[liquid]]. Designed by Shopify specifically so storefront merchants couldn't execute arbitrary code.
- **You write them, in Python** → [Jinja2](https://jinja.palletsprojects.com/) (no article yet — canonical landing page).
- **You write them, in Rust** → [Tera](https://keats.github.io/tera/) (Jinja-shaped; what Zola SSG uses).
- **You write them, in JS** → [Handlebars](https://handlebarsjs.com/), [EJS](https://ejs.co/), or just JSX if you're already on React.

The deciding axis is **safety vs. expressiveness**. Liquid is intentionally restricted; Jinja and Tera let you call into the host language and have escape hatches that have caused real RCEs when fed user input.

## Articles

- [[liquid]] — Shopify's safe template language; what Jekyll and `liquidjs` use.

## Keywords

`#web-templating` `#liquid` `#jinja` `#handlebars` `#mustache` `#tera`
