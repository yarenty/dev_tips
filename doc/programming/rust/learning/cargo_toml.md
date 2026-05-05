---
title: Cargo.toml
main_link: 
keywords: [cargo-toml, learning, rust, programming, cargo, serde, tricks, toml]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/learning/_must_have.md` by `article_split.py`. Heading: **Cargo.toml**.

# Cargo.toml

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[_tricks]] — println! speedup 500% _(score 32.4)_
- [[json]] — JSON _(score 23.6)_
- [[_todo_ideas]] — Move blokchain from python to rust _(score 23.5)_
- [[debug]] — Debug _(score 14.7)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#cargo-toml` `#learning` `#rust` `#programming` `#cargo` `#serde` `#trick` `#toml`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Cargo.toml

```toml
[dependencies]
chrono = "0.4"
log = "0.4.16"
simplelog = "0.12.0"
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
```


remove 50% size of binary by:
```toml
[profile.release]
strip = true # remove if using bloat
panic = "abort" # Strip expensive panic clean-up logic
codegen-units = 1 # Compile crates one after another so the compiler can optimize better
lto = true # Enables link to optimizations
opt-level = "s" # Optimize for binary size - try "z" 

```


speedup debugging builds

```toml

# Enable a small amount of optimization in debug mode
[profile.dev]
opt-level = 1

# Enable high optimizations for dependencies, but not for our code:
[profile.dev.package."*"]
opt-level = 3

```


Must have traits:

```rust
#[cfg(feature="serde")]
use serde::{Deserialize, Serialize};


#[derive(Debug, Clone, Default, PartialEq)]
#[cfg_attr(feature="serde", derive(Deserialize, Serialize))]
pub struct UpdateUserRequest {
    /// <p>The user's email address.</p>
    #[doc(hidden)]
    pub email: std::option::Option<std::string::String>,
    /// <p>The user's first name.</p>
    #[doc(hidden)]
    pub first_name: std::option::Option<std::string::String>,
    /// <p>The user's last name.</p>
    #[doc(hidden)]
    pub last_name: std::option::Option<std::string::String>,
    /// <p>The user's password.</p>
    #[doc(hidden)]
    #[cfg_attr(feature="serde" serde(skip)]
    pub password: std::option::Option<std::string::String>,
}


fn main() {
 
 let s = "{ \"email\": \"aa@aa.a\", \"first_name\": \"John\",  \"last_name\": \"Travolta\" }";
 let u :UpdateUserRequest = serde_json::from_str(s).unwrap();
 
}
```

Serde - optional  - switched in cargo
```toml

[dependencies]
serde= { version = "1.0", features=["derive"], optional = true}
serde_json = "1.0"

[features]
serde = ["dep:serde"]
```


TRICK: automatic test for send & synd :

```rust

fn is_normal<T: Sized + Send + Sync + Unpin>() {}

#[test]
fn normal_types() {
 is_normal<UpdateUserRequest>();
}

```
