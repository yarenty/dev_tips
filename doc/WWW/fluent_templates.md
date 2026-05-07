---
title: fluent-templates — Fluent i18n for Rust templates
main_link: https://github.com/XAMPPRocky/fluent-templates
keywords: [fluent-templates, fluent, i18n, l10n, rust, handlebars, tera, mozilla]
status: reviewed
review_date: 2026/05/03
---

# fluent-templates — Fluent i18n for Rust templates

**Main link:** <https://github.com/XAMPPRocky/fluent-templates>

## Summary

`fluent-templates` is a Rust crate by **XAMPPRocky** (Erin Power) that wires Mozilla's **Project Fluent** localisation system into popular templating engines (handlebars, tera). It exposes a high-level **loader API** that picks the right Fluent strings for a request based on simple language negotiation (`Accept-Language`-style fallback chains), plus a generic `FluentLoader` struct with optional integrations so templates can do `{{ fluent "key" }}` with no boilerplate.

## Insight

If you're building a multilingual Rust web app — Loco, Axum, Actix, Rocket, Yew, anything — and you've used Mozilla's Fluent localisation files (`.ftl`) before, this is the bridge that makes them usable from your templates with one line of glue code. Fluent itself is the right choice over GNU gettext for new code because it handles plurals, gender, and selectors gracefully; the only hard part has historically been wiring it into templates, which is what this crate solves.

If you're not committed to Fluent yet, evaluate the trade-off first: GNU gettext is older and more widely supported (browsers, native apps, every CMS), while Fluent has nicer ergonomics for complex linguistic patterns. Fluent's adoption outside Mozilla is still modest in 2025-2026.

## Similar / related topics

- **Project Fluent** — the underlying localisation system from Mozilla: <https://projectfluent.org/>
- **`fluent` crate** — lower-level Rust bindings to Fluent (this crate sits on top): <https://github.com/projectfluent/fluent-rs>
- **`gettext` / `gettext-rs`** — the GNU alternative; older, more widely used.
- **Tera / Handlebars** — the template engines `fluent-templates` integrates with.
- [[loco_rust_on_rails|Loco]] — a Rails-ish Rust web framework that pairs naturally with this crate.

## Internal links

- [[loco_rust_on_rails]] — Rails-ish web framework where this slots in as the i18n layer.
- [[ssg]] — Static-site generators (Zola also uses Tera and could benefit from Fluent).

## Keywords

`#fluent-templates` `#fluent` `#i18n` `#l10n` `#rust` `#handlebars` `#tera` `#mozilla`

## References / raw notes

- Repo: <https://github.com/XAMPPRocky/fluent-templates>
- crates.io: <https://crates.io/crates/fluent-templates>
- Project Fluent home: <https://projectfluent.org/>
- Fluent syntax guide: <https://projectfluent.org/fluent/guide/>

### What the crate gives you

Project description (verbatim):

> fluent-templates lets you to easily integrate Fluent localisation into your Rust application or library. It does this by providing a high level "loader" API that loads fluent strings based on simple language negotiation, and the FluentLoader struct which is a Loader agnostic container type that comes with optional trait implementations for popular templating engines such as handlebars or tera that allow you to be able to use your localisations in your templates with no boilerplate.

### Minimal usage sketch

```rust
use fluent_templates::{static_loader, Loader};

static_loader! {
    static LOCALES = {
        locales: "./locales",
        fallback_language: "en-US",
    };
}

fn main() {
    let langid = "es-ES".parse().unwrap();
    let msg = LOCALES.lookup(&langid, "greeting");
    println!("{}", msg);   // "Hola, mundo"
}
```

With Tera, `LOCALES` registers as a Tera function and templates can do `{{ fluent("greeting") }}` directly.
