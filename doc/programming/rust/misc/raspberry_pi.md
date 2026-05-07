---
title: Rust on Raspberry Pi
main_link: https://docs.rust-embedded.org/book/
keywords: [raspberry-pi, rust, rppal, embedded, cross-compile, pico, rp2040]
status: reviewed
---

# Rust on Raspberry Pi

**Main link:** <https://docs.rust-embedded.org/book/>

## Summary

The Raspberry Pi family splits cleanly into two very different Rust stories:

1. **Linux-class Pi** (Pi 3, 4, 5, Zero 2 W, …): Rust runs as a normal Linux userspace binary. You either build natively on the Pi, or cross-compile from your laptop using [`cross`](https://github.com/cross-rs/cross) or [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild). GPIO/I²C/SPI/PWM access goes through [`rppal`](https://github.com/golemparts/rppal) (Rust Peripheral Access Library), which talks `/dev/gpiomem` and friends.
2. **Microcontroller Pi** (RP2040, RP2350 — i.e. Pi Pico, Pico 2): a Cortex-M0+/Cortex-M33 you flash directly. The Embassy ecosystem covers it via [`embassy-rp`](https://github.com/embassy-rs/embassy/tree/main/embassy-rp); RTIC works via `rp2040-hal`. The [Rust Embedded Book](https://docs.rust-embedded.org/book/) is the canonical entry point for this side.

This article is a pointer page rather than a tutorial — pick the side you're on and follow the appropriate ecosystem map.

## Insight

Some practical guidance distilled from common Pi-Rust footguns:

- **Cross-compile target for Pi 3/4/5 64-bit**: `aarch64-unknown-linux-gnu`. For 32-bit Raspberry Pi OS on Pi 2/3/4: `armv7-unknown-linux-gnueabihf`. For Pi Zero (W) / original: `arm-unknown-linux-gnueabihf` (older ARMv6 — `arm-unknown-linux-gnueabihf` not `armv7-…-gnueabihf`).
- **Use `cross` or `cargo-zigbuild`** rather than fighting `gcc-aarch64-linux-gnu` toolchain installs by hand. `cross` runs the build in a Docker container with the right sysroot baked in; `cargo-zigbuild` uses `zig cc` as the linker (no Docker, faster).
- **`rppal` needs the `gpio` group** on the Pi, or running as root. It does not need root if your user is in the `gpio`, `i2c`, `spi` groups (the default `pi` user usually is).
- **Pi cluster / k8s**: the Pi 4 / 5 are good [[../../../kubernetes/k3s|k3s]] nodes; Rust binaries on Pi clusters are a popular pattern for edge-IoT workloads. See `iot/` for the pipeline side.
- **`linux-embedded-hal`** lets you reuse drivers written against `embedded-hal` (originally aimed at MCUs) on a Linux Pi — handy for sensor crates that target both Pico and Pi 4.

## Similar / related topics

- [Rust Embedded Book](https://docs.rust-embedded.org/book/) — canonical no-`std` tutorial.
- [`rppal`](https://github.com/golemparts/rppal) — Pi GPIO/I²C/SPI/UART/PWM crate.
- [`cross`](https://github.com/cross-rs/cross) / [`cargo-zigbuild`](https://github.com/rust-cross/cargo-zigbuild) — cross-compile toolchains.
- [`linux-embedded-hal`](https://github.com/rust-embedded/linux-embedded-hal) — `embedded-hal` traits over Linux sysfs.
- [`rpi-rs`](https://github.com/rpi-rs) — community org for various Pi-specific crates.

## Internal links

- [[awesome_embedded_in_rust]] — Rust Embedded WG curated list (covers Pico via `rp-rs`, etc.).
- [[embassy]] — async embedded framework; Pico support via `embassy-rp`.
- [[rtic]] — interrupt-priority alternative; Pico via `rp2040-hal`.
- [[../../../iot/README|iot]] — vault-wide IoT section (Pis as edge nodes).
- [[../../../kubernetes/k3s|k3s]] — Kubernetes on Pi clusters.

## Keywords

`#raspberry-pi` `#rust` `#rppal` `#embedded` `#cross-compile` `#pico` `#rp2040`

## References / raw notes

- The Rust Embedded Book: <https://docs.rust-embedded.org/book/> — "An introductory book about using Rust on bare-metal embedded systems, such as microcontrollers."
- Rust Embedded Working Group: <https://github.com/rust-embedded>
- `awesome-embedded-rust` directory: <https://github.com/rust-embedded/awesome-embedded-rust>
- `rppal` (Pi peripheral access on Linux): <https://github.com/golemparts/rppal>
- `cross` cross-compile: <https://github.com/cross-rs/cross>
- `cargo-zigbuild`: <https://github.com/rust-cross/cargo-zigbuild>
- `rp-rs` (Pico HAL org): <https://github.com/rp-rs>
- Embassy RP HAL: <https://github.com/embassy-rs/embassy/tree/main/embassy-rp>
