---
title: indicatif
main_link: https://github.com/console-rs/indicatif
keywords: [indicatif, rust, cli, progress, spinners, console-rs, multiprogress]
status: reviewed
review_date: 2026/05/03
---

# indicatif

**Main link:** <https://github.com/console-rs/indicatif>

## Summary

`indicatif` is the de-facto Rust progress-bar / spinner library, from the `console-rs` family (sibling of [[dialoguer]] and `console`). It gives you `ProgressBar`, `ProgressStyle`, `MultiProgress`, spinners, byte/duration formatters, and templated prefixes/suffixes — everything you need to make a long-running CLI feel responsive. It plays well with iterators (`my_iter.progress_count(N)`) and with parallel workloads.

## Insight

Reach for `indicatif` whenever a CLI does enough work that the user might wonder if it's frozen. Three patterns cover most uses:

1. **Single deterministic bar** — you know the total: `ProgressBar::new(total)`, then `.inc(1)` per step.
2. **Indeterminate spinner** — you don't know the total: `ProgressBar::new_spinner()` + `.enable_steady_tick(...)`.
3. **`MultiProgress` for parallel jobs** — the killer feature. Multiple bars stacked, updated independently from worker threads. Combine with [`rayon`](https://crates.io/crates/rayon) or [[../concurrency/tokio|tokio]] tasks; each task gets a `pb = mp.add(ProgressBar::new(...))`.

Gotchas:

- **Don't `println!` from inside a progress region** — it'll fight the bar. Use `pb.println(...)` / `pb.suspend(|| ...)` or set up an `IndicatifLayer` for `tracing`.
- **Tick rate matters**: too fast and you waste CPU; too slow and the bar feels laggy. Default is fine for most cases.
- **Templates are powerful**: `{spinner} {wide_bar} {pos}/{len} {eta}` etc. — see [the template docs](https://docs.rs/indicatif/latest/indicatif/#templates).
- The closely related crate [`indicatif-log-bridge`](https://crates.io/crates/indicatif-log-bridge) lets `log` output coexist with bars without corruption; `tracing-indicatif` does the same for tracing.

For users coming from Python's `tqdm`, [`kdam`](https://crates.io/crates/kdam) is a closer-feeling port; `indicatif` has the deeper Rust-ecosystem integration.

## Similar / related topics

- [[dialoguer]] — sibling crate; pair them for "ask user → run with progress".
- [`kdam`](https://crates.io/crates/kdam) — `tqdm`-shaped Rust port.
- [`linya`](https://crates.io/crates/linya) — lightweight multi-bar alternative.
- [`tracing-indicatif`](https://crates.io/crates/tracing-indicatif) — bridge between `tracing` spans and progress bars.

## Internal links
- [[../cli/README|Rust CLI]]
- [[dialoguer]]
- [[../concurrency/tokio|tokio]] — common for parallel jobs that drive `MultiProgress`.

## Keywords

`#rust` `#cli` `#indicatif` `#progress` `#spinner` `#multiprogress` `#console-rs`

## References / raw notes

- Repo: <https://github.com/console-rs/indicatif>
- Crate: <https://crates.io/crates/indicatif>

> Rust library for indicating progress in command line applications to users.
>
> This currently primarily provides progress bars and spinners as well as basic color support, but there are bigger plans for the future of this!
