---
title: Embassy
main_link: https://github.com/embassy-rs/embassy
keywords: [embassy, misc, rust, programming, usb, async, task, hals]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/misc/raspberry_pi.md` by `article_split.py`. Heading: **Embassy**.

# Embassy

**Main link:** <https://github.com/embassy-rs/embassy>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rtic]] — RTIC _(score 28)_
- [[raspberry_pi]] — Rust Embedded - book _(score 28)_
- [[hound]] — Hound _(score 18)_
- [[daktilo]] — Daktilo _(score 18)_
- [[claxon]] — Claxon _(score 18)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#embassy` `#misc` `#rust` `#programming` `#usb` `#async` `#tasks` `#hals`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Embassy

https://github.com/embassy-rs/embassy

https://embassy.dev/dev/index.html


Embassy is the next-generation framework for embedded applications. Write safe, correct and energy-efficient embedded code faster, using the Rust programming language, its async facilities, and the Embassy libraries.


Rust + async ❤️ embedded


Rust's async/await allows for unprecedently easy and efficient multitasking in embedded systems. Tasks get transformed at compile time into state machines that get run cooperatively. It requires no dynamic memory allocation, and runs on a single stack, so no per-task stack size tuning is required. It obsoletes the need for a traditional RTOS with kernel context switching, and is faster and smaller than one!


Hardware Abstraction Layers - HALs implement safe, idiomatic Rust APIs to use the hardware capabilities, so raw register manipulation is not needed. The Embassy project maintains HALs for select hardware, but you can still use HALs from other projects with Embassy.

embassy-stm32, for all STM32 microcontroller families.
embassy-nrf, for the Nordic Semiconductor nRF52, nRF53, nRF91 series.
Time that Just Works - No more messing with hardware timers. embassy_time provides Instant, Duration and Timer types that are globally available and never overflow.

Real-time ready - Tasks on the same async executor run cooperatively, but you can create multiple executors with different priorities, so that higher priority tasks preempt lower priority ones. See the example.

Low-power ready - Easily build devices with years of battery life. The async executor automatically puts the core to sleep when there's no work to do. Tasks are woken by interrupts, there is no busy-loop polling while waiting.

Networking - The embassy-net network stack implements extensive networking functionality, including Ethernet, IP, TCP, UDP, ICMP and DHCP. Async drastically simplifies managing timeouts and serving multiple connections concurrently.

Bluetooth - The nrf-softdevice crate provides Bluetooth Low Energy 4.x and 5.x support for nRF52 microcontrollers.

LoRa - embassy-lora supports LoRa networking on STM32WL wireless microcontrollers and Semtech SX127x transceivers.

USB - embassy-usb implements a device-side USB stack. Implementations for common classes such as USB serial (CDC ACM) and USB HID are available, and a rich builder API allows building your own.

Bootloader and DFU - embassy-boot is a lightweight bootloader supporting firmware application upgrades in a power-fail-safe way, with trial boots and rollbacks.
