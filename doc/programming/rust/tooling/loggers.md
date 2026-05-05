---
title: Loggers
main_link: https://github.com/tokio-rs/tracing
keywords: [loggers, rust, log4rs, tracing]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Loggers

**Main link:** <https://github.com/tokio-rs/tracing>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tracing]] — Tracing _(score 26.0)_
- [[loki]] — Loki _(score 17.1)_
- [[programming/rust/tooling/tauri|tauri]] — TAURI _(score 17.1)_
- [[debug]] — Debug _(score 17.1)_
- [[rtic]] — RTIC _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#loggers` `#tooling` `#rust` `#programming` `#log4rs` `#tracing` `#simplelog` `#env`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

## Tracing


https://github.com/tokio-rs/tracing




## Log

```rust

use log::{info,warn, debug, trace};
#[macro_use] extern crate log;
extern crate simplelog;

use simplelog::*;


#[tokio::main]
async fn main() -> Result<()> {
    // terminal only output:
    TermLogger::init(LevelFilter::Debug, Config::default(), TerminalMode::Mixed, ColorChoice::Auto).unwrap();

    // both terminal and log file
    CombinedLogger::init(
        vec![
            TermLogger::new(LevelFilter::Warn, Config::default(), TerminalMode::Mixed, ColorChoice::Auto),
            WriteLogger::new(LevelFilter::Info, Config::default(), File::create("my_rust_binary.log").unwrap()),
        ]
    ).unwrap();

}


```

## Simplelog

https://github.com/drakulix/simplelog.rs



## Log4rs
https://docs.rs/log4rs/latest/log4rs/


## Env_logger
https://crates.io/crates/env_logger

```toml
env_logger = "0.9.0"
```


```rust
#[macro_use]
extern crate log;

fn main() {
    env_logger::init();

    info!("starting up");

    // ...
}
```
