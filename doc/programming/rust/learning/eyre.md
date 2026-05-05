---
title: eyre
main_link: https://github.com/awslabs/aws-sdk-rust
keywords: [eyre, learning, rust, programming, file, crates, env, dotenv]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/learning/_must_have.md` by `article_split.py`. Heading: **eyre**.

# eyre

**Main link:** <https://github.com/awslabs/aws-sdk-rust>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#eyre` `#learning` `#rust` `#programming` `#file` `#crates` `#env` `#dotenv`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# eyre
looks like need to check switch to this one - proper error display in runtime
 use this together with thiserror:
 - this error for own internal errors
 - eyre for global main or as in example below

https://crates.io/crates/color-eyre

```rust
use eyre::{eyre, Result};

let opt: Option<()> = None;
let result: Result<()> = opt.ok_or_else(|| eyre!("new error message"));

```


## Environment variables
from .env file
The easiest and most common usage consists on calling dotenv::dotenv when the application starts, which will load environment variables from a file named .env in the current directory or any of its parents; after that, you can just call the environment-related method you need as provided by std::os.

If you need finer control about the name of the file or its location, you can use the from_filename and from_path methods provided by the crate.

dotenv_codegen provides the dotenv! macro, which behaves identically to env!, but first tries to load a .env file at compile time.

Examples
A .env file looks like this:
```yaml
# a comment, will be ignored
REDIS_ADDRESS=localhost:6379
MEANING_OF_LIFE=42
```
You can optionally prefix each line with the word export, which will conveniently allow you to source the whole file on your shell.

A sample project using Dotenv would look like this:
```rust
extern crate dotenv;

use dotenv::dotenv;
use std::env;

fn main() {
dotenv().ok();

    for (key, value) in env::vars() {
        println!("{}: {}", key, value);
    }
}
```
Variable substitution
It's possible to reuse variables in the .env file using $VARIABLE syntax. The syntax and rules are similar to bash ones, here's the example:

## Error handling

https://crates.io/crates/thiserror

```toml
thiserror = "1"
```




## HTTP Request

https://docs.rs/reqwest/latest/reqwest/

The reqwest crate provides a convenient, higher-level HTTP Client.

It handles many of the things that most people just expect an HTTP client to do for them.

- Async and blocking Clients
- Plain bodies, JSON, urlencoded, multipart
- Customizable redirect policy
- HTTP Proxies
- Uses system-native TLS
- Cookies

```rust
let body = reqwest::get("https://www.rust-lang.org")
    .await?
    .text()
    .await?;

println!("body = {:?}", body);


let client = reqwest::Client::new();
let res = client.post("http://httpbin.org/post")
.body("the exact body that is sent")
.send()
.await?;


// This will POST a body of `foo=bar&baz=quux`
let params = [("foo", "bar"), ("baz", "quux")];
let client = reqwest::Client::new();
let res = client.post("http://httpbin.org/post")
.form(&params)
.send()
.await?;


// This will POST a body of `{"lang":"rust","body":"json"}`
let mut map = HashMap::new();
map.insert("lang", "rust");
map.insert("body", "json");

let client = reqwest::Client::new();
let res = client.post("http://httpbin.org/post")
.json(&map)
.send()
.await?;
```


## lazy_static

https://docs.rs/lazy_static/latest/lazy_static/

A macro for declaring lazily evaluated statics.

Using this macro, it is possible to have statics that require code to be executed at runtime in order to be initialized. This includes anything requiring heap allocations, like vectors or hash maps, as well as anything that requires function calls to be computed.

```rust
#[macro_use]
extern crate lazy_static;

use std::collections::HashMap;

lazy_static! {
    static ref HASHMAP: HashMap<u32, &'static str> = {
        let mut m = HashMap::new();
        m.insert(0, "foo");
        m.insert(1, "bar");
        m.insert(2, "baz");
        m
    };
    static ref COUNT: usize = HASHMAP.len();
    static ref NUMBER: u32 = times_two(21);
}

fn times_two(n: u32) -> u32 { n * 2 }

fn main() {
    println!("The map has {} entries.", *COUNT);
    println!("The entry for `0` is \"{}\".", HASHMAP.get(&0).unwrap());
    println!("A expensive calculation on a static results in: {}.", *NUMBER);
}

```




## Chrono

https://crates.io/crates/chrono


## Tokio

https://tokio.rs/

https://docs.rs/tokio/1.18.0/tokio/




## AMAZON SDK

https://github.com/awslabs/aws-sdk-rust
