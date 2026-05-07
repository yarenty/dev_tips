---
title: Embassy — async embedded Rust framework
main_link: https://embassy.dev/
keywords: [embassy, rust, embedded, async, no-std, executor, hal]
status: reviewed
---

# Embassy — async embedded Rust framework

**Main link:** <https://embassy.dev/>

## Summary

Embassy is a modern async-first framework for embedded Rust. It bundles a no-`std` async executor (`embassy-executor`), a hardware-agnostic time abstraction (`embassy-time`), HALs for major MCU families (`embassy-stm32`, `embassy-nrf`, `embassy-rp` for the RP2040/RP2350, `embassy-imxrt`, …), a TCP/IP stack (`embassy-net`), USB device stack (`embassy-usb`), Bluetooth Low Energy via `nrf-softdevice`, LoRa via `embassy-lora`, and a power-fail-safe bootloader (`embassy-boot`). Tasks are compiled into state machines and cooperatively scheduled on a single stack — no per-task stack tuning, no kernel context-switch overhead.

## Insight

Embassy is the modern alternative to a traditional RTOS for Cortex-M (and increasingly RISC-V) embedded work. Reach for it when your firmware is naturally I/O-bound — multiple peripherals, timers, network, USB, BLE — and you'd otherwise be juggling interrupt handlers, ring buffers, and state machines by hand. The async/await transformation gives you the readability of "linear" code with no allocator and very low overhead. Practical notes:

- **Embassy vs [[rtic]]**: Embassy is async-first (cooperative; one task waits on a future, scheduler runs the next ready one); RTIC is interrupt-priority-first (statically-prioritised hardware tasks, race-free shared resources via priority ceilings). Both can coexist in the same binary, and both are recommended in different problem shapes — RTIC for hard-real-time deterministic latency, Embassy for I/O-rich firmware.
- **HAL story**: Embassy's HALs are *async-native* — `i2c.write(...).await`, not "kick off a DMA + register an interrupt callback." If your chip family doesn't have an Embassy HAL, you can still use any `embedded-hal-async`–compatible HAL with the Embassy executor.
- **Power efficiency**: the executor automatically `wfi`s the core when the run-queue is empty; tasks resume on interrupt. Years-of-battery-life devices are a stated design goal, not aspiration.
- **Boards covered well today**: STM32 (every family), Nordic nRF52/nRF53/nRF91, Raspberry Pi RP2040/RP2350, ESP32 (via `esp-hal` + Embassy executor), i.MX RT, and more landing.

## Similar / related topics

- [[rtic]] — sibling concurrency framework, interrupt-priority-based.
- [`embedded-hal-async`](https://github.com/rust-embedded/embedded-hal) — the trait crate Embassy HALs implement.
- [`smoltcp`](https://github.com/smoltcp-rs/smoltcp) — the no-`std` TCP/IP stack underneath `embassy-net`.
- [Drone OS](https://www.drone-os.com/) — alternative async embedded executor.
- [Tock](https://www.tockos.org/) — a more traditional embedded OS in Rust.

## Internal links

- [[rtic]] — sibling, the interrupt-priority alternative.
- [[awesome_embedded_in_rust]] — entry-point reading list.
- [[raspberry_pi]] — Embassy supports the Raspberry Pi Pico (RP2040) via `embassy-rp`.
- [[../../../iot/drogue|drogue]] — Drogue's device side targets Embassy-style firmware.
- [[../../../iot/README|iot]] — vault-wide IoT section.

## Keywords

`#embassy` `#rust` `#embedded` `#async` `#no-std` `#executor` `#hal`

## References / raw notes

- Site: <https://embassy.dev/>
- Repo: <https://github.com/embassy-rs/embassy>
- Docs: <https://embassy.dev/book/>
- Pitch: Rust + async ❤️ embedded — single-stack cooperative tasks compiled to state machines, no allocator, no per-task stack tuning, obsoletes traditional RTOS context switching for the I/O-bound case.
- Components: `embassy-executor`, `embassy-time`, `embassy-net` (TCP/IP, ICMP, UDP, DHCP), `embassy-usb`, `embassy-boot`, `embassy-lora`, `nrf-softdevice` (BLE on nRF52).
- HALs: `embassy-stm32`, `embassy-nrf`, `embassy-rp`, plus community ports.
