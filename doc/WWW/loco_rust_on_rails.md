# Loco


oposite to ssg ;-)


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