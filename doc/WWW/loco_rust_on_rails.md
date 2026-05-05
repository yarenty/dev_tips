---
title: Loco
main_link: https://github.com/loco-rs/loco
keywords: [loco-rust-on-rails, rails, storage, rust, background]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Loco

**Main link:** <https://github.com/loco-rs/loco>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#loco-rust-on-rails` `#rails` `#storage` `#rust` `#background`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Loco


opposite to ssg ;-)

## [Dioxus -check](../programming/rust/gui/gui.mdi.md)

https://www.youtube.com/watch?v=7utPutDORb4

https://jondot.medium.com/introduction-to-loco-the-rust-on-rails-7322a9095d7d

https://loco.rs/

https://github.com/loco-rs/loco



## What's Loco?
Loco is strongly inspired by Rails. If you know Rails and Rust, you'll feel at home. If you only know Rails and new to Rust, you'll find Loco refreshing. We do not assume you know Rails.

For a deeper dive into how Loco works, including detailed guides, examples, and API references, check out our documentation website.

## Features of Loco:
- Convention Over Configuration: Similar to Ruby on Rails, Loco emphasizes simplicity and productivity by reducing the need for boilerplate code. It uses sensible defaults, allowing developers to focus on writing business logic rather than spending time on configuration.

- Rapid Development: Aim for high developer productivity, Loco’s design focuses on reducing boilerplate code and providing intuitive APIs, allowing developers to iterate quickly and build prototypes with minimal effort.

- ORM Integration: Model your business with robust entities, eliminating the need to write SQL. Define relationships, validation, and custom logic directly on your entities for enhanced maintainability and scalability.

- Controllers: Handle web requests parameters, body, validation, and render a response that is content-aware. We use Axum for the best performance, simplicity, and extensibility. Controllers also allow you to easily build middlewares, which can be used to add logic such as authentication, logging, or error handling before passing requests to the main controller actions.

- Views: Loco can integrate with templating engines to generate dynamic HTML content from templates.

- Background Jobs: Perform compute or I/O intensive jobs in the background with a Redis backed queue, or with threads. Implementing a worker is as simple as implementing a perform function for the Worker trait.

- Scheduler: Simplifies the traditional, often cumbersome crontab system, making it easier and more elegant to schedule tasks or shell scripts.

- Mailers: A mailer will deliver emails in the background using the existing loco background worker infrastructure. It will all be seamless for you.

- Storage: In Loco Storage, we facilitate working with files through multiple operations. Storage can be in-memory, on disk, or use cloud services such as AWS S3, GCP, and Azure.

- Cache: Loco provides an cache layer to improve application performance by storing frequently accessed data.
