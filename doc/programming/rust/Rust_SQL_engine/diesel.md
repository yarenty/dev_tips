# diesel

https://crates.io/crates/diesel

Diesel - A safe, extensible ORM and Query Builder for Rust
Diesel is the most productive way to interact with databases in Rust because of its safe and composable abstractions over queries.


https://github.com/diesel-rs/diesel/tree/2.0.x


Diesel gets rid of the boilerplate for database interaction and eliminates runtime errors without sacrificing performance. It takes full advantage of Rust's type system to create a low overhead query builder that "feels like Rust."

Supported databases:

- PostgreSQL
- MySQL
-SQLite

You can configure the database backend in Cargo.toml:

```toml
[dependencies]
diesel = { version = "<version>", features = ["<postgres|mysql|sqlite>"] }
```

## Getting Started
Find our extensive Getting Started tutorial at https://diesel.rs/guides/getting-started. Guides on more specific features are coming soon.