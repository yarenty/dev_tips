---
title: Loco — Rails-style web framework for Rust
main_link: https://loco.rs/
keywords: [loco, rust, web-framework, rails, axum, sea-orm, full-stack]
status: reviewed
review_date: 2026/05/03
---

# Loco — Rails-style web framework for Rust

**Main link:** <https://loco.rs/>

## Summary

**Loco** is a full-stack Rust web framework deliberately patterned on Ruby on Rails — convention over configuration, generators, ORM-baked-in, controllers + views + workers + mailers + scheduler + storage + cache, all in one box. Built on top of **Axum** (HTTP), **SeaORM** (DB / migrations), and **sidekiq.rs / Redis** (background jobs). Created by Dotan Nahum (`jondot`); MIT-licensed; the slogan is literally "Rust on Rails".

## Insight

The right choice when you're a solo dev or small team building a **server-rendered web app** in Rust and don't want to assemble Axum + an ORM + workers + auth + mailers from individual crates yourself. The Rails-style generators (`loco generate model post title:string`) and the convention layout mean you can be productive in minutes instead of days. Where Loco shines is the *feature breadth*: it ships background workers, scheduling, mailers, file storage abstraction (S3/GCS/Azure/local), cache layer, and authentication out of the box — most other Rust frameworks make you pick those individually.

The trade-offs to know: (1) it's the **opposite of static-site generators** like [[ssg|Zola]] — full server rendering and DB access; (2) it's still relatively young (pre-1.0 as of 2025); (3) deep customisation means leaving the convention path, which can be painful; (4) the Rails-developer audience is small (most Rails devs aren't switching to Rust); the actual audience is **Rust devs who want a productive web framework**.

## Similar / related topics

- **Axum** — the lower-level HTTP framework Loco builds on; reach for Axum directly if you want full control.
- **Actix Web / Rocket / Warp** — competing Rust web frameworks; smaller scope (HTTP only, no ORM/workers).
- **Ruby on Rails** — the original; Loco's API is a deliberate homage.
- **Phoenix (Elixir)** — equivalent "batteries-included" framework for the BEAM.
- [[ssg]] — opposite end of the spectrum (static generation vs full server-rendered app).

## Internal links

- [[ssg]] — Static-site-generation alternative (when you don't need a server).
- [[fluent_templates]] — i18n layer that pairs well with Loco's view layer.
- [[apache]] / [[ssl]] — Possible deployment substrates.

## Keywords

`#loco` `#rust` `#web-framework` `#rails` `#axum` `#sea-orm` `#full-stack`

## References / raw notes

- Project site: <https://loco.rs/>
- Repo: <https://github.com/loco-rs/loco>
- Author's intro article (Jondot): <https://jondot.medium.com/introduction-to-loco-the-rust-on-rails-7322a9095d7d>
- YouTube intro: <https://www.youtube.com/watch?v=7utPutDORb4>

### Loco features at a glance

| Layer | What you get out of the box |
|-------|-----------------------------|
| HTTP | **Axum** routing + middleware |
| ORM / migrations | **SeaORM** with generators |
| Controllers | Request body / params / validation, content-aware response |
| Views | Tera templating; pairs with [[fluent_templates]] for i18n |
| Background jobs | Redis-backed queue (default) or in-process threads; `Worker::perform` trait |
| Scheduler | Cron-like, but Rust-native and elegant |
| Mailers | Async email delivery via the same worker infrastructure |
| Storage | Pluggable: in-memory, local disk, S3, GCS, Azure |
| Cache | Built-in cache layer (Redis or in-memory) |
| Auth | JWT, sessions, password hashing |
| Generators | `loco generate {model,controller,scaffold,migration,worker,mailer,scheduler}` |

### Quick start (paraphrased from the docs)

```shell
# Install the CLI (cargo)
cargo install loco-cli

# New app:
loco new myapp
cd myapp

# Generate a scaffold:
cargo loco generate scaffold post title:string content:text

# Run migrations + start the server:
cargo loco db migrate
cargo loco start
```

### Why "convention over configuration" matters

In Rails this means a `Post` model maps to a `posts` table and is served by `PostsController` whose `new`/`create`/`edit`/`update`/`destroy` actions render `app/views/posts/*.html.erb` — without any wiring. Loco mirrors that pattern in Rust: `models/post.rs` + `controllers/posts.rs` + `views/posts/*.tera` are wired together by the framework's startup convention. If you've used Rails this is immediately familiar; if you haven't, the docs cover the conventions explicitly.
