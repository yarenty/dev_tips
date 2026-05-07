---
title: RTIC — Real-Time Interrupt-driven Concurrency
main_link: https://rtic.rs/
keywords: [rtic, rust, embedded, real-time, concurrency, cortex-m, scheduler]
status: reviewed
review_date: 2026/05/03
---

# RTIC — Real-Time Interrupt-driven Concurrency

**Main link:** <https://rtic.rs/>

## Summary

RTIC (Real-Time Interrupt-driven Concurrency, formerly *Real-Time For the Masses*) is a concurrency framework for embedded Rust originally targeting ARM Cortex-M, now expanding to RISC-V. Tasks are defined as functions, given a static priority, and dispatched directly by hardware interrupts. Shared resources are made race-free by the **Stack Resource Policy** (priority ceilings) — the framework statically proves at compile time that lower-priority tasks cannot preempt while a higher-priority task is touching a shared resource, so locks become near-zero-cost critical sections. The result is hard-real-time scheduling with deterministic latency and no allocator.

## Insight

RTIC is the natural choice when you have a small, well-defined set of recurrent jobs (read sensor at X Hz, drive PWM, respond to interrupt within Y µs) and the *latency* of each job matters more than the *throughput* of many jobs. Practical guidance:

- **RTIC vs [[embassy]]**: RTIC owns the scheduler and uses static priorities + interrupts; Embassy is async/await + cooperative. They solve overlapping but distinct problems and can interoperate (RTIC tasks can `await`, and Embassy can be hosted inside an RTIC software task).
- **No dynamic dispatch, no allocator, no async runtime cost** — the macro generates a hand-written-quality dispatcher at compile time.
- **Software tasks** (run via "dispatchers" — pending unused interrupts as virtual workers) extend the model beyond just hardware interrupts.
- **The 2.0 release** introduced `async` support inside RTIC tasks, partially blurring the line with Embassy.
- The org/repo moved to `rtic-rs/rtic` (the older `rtic-rs/cortex-m-rtic` URL still 301-redirects).

## Similar / related topics

- [[embassy]] — sibling embedded framework, async-first.
- [`cortex-m`](https://github.com/rust-embedded/cortex-m) — the foundational ARM Cortex-M crate RTIC builds on.
- [Tock](https://www.tockos.org/) — embedded OS with kernel/process separation, complementary positioning.
- [FreeRTOS](https://www.freertos.org/) — the C-world traditional comparison point.
- [Drone OS](https://www.drone-os.com/) — alternative async embedded executor.

## Internal links

- [[embassy]] — sibling, the async-first alternative.
- [[awesome_embedded_in_rust]] — entry-point reading list.
- [[raspberry_pi]] — Raspberry Pi Pico (RP2040) is supported via `cortex-m-rtic` + `rp2040-hal`.
- [[../../../iot/README|iot]] — vault-wide IoT section (RTIC firmware often pairs with IoT pipelines).

## Keywords

`#rtic` `#rust` `#embedded` `#real-time` `#concurrency` `#cortex-m` `#scheduler`

## References / raw notes

- Site / book: <https://rtic.rs/>
- Repo: <https://github.com/rtic-rs/rtic> (older URL <https://github.com/rtic-rs/cortex-m-rtic> redirects)
- crates.io: <https://crates.io/crates/cortex-m-rtic> / <https://crates.io/crates/rtic>
- docs.rs: <https://docs.rs/cortex-m-rtic/latest/rtic/>
- Tagline: "A concurrency framework for building real-time systems." (Formerly Real-Time For the Masses.)
