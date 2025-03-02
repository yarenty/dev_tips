# Rust Embedded - book
https://docs.rust-embedded.org/book/

An introductory book about using the Rust Programming Language on "Bare Metal" embedded systems, such as Microcontrollers.



# Awesome embedded in rust

https://github.com/rust-embedded/awesome-embedded-rust



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



# RTIC

https://crates.io/crates/cortex-m-rtic

https://docs.rs/cortex-m-rtic/latest/rtic/

https://rtic.rs/

https://github.com/rtic-rs/cortex-m-rtic



A concurrency framework for building real-time systems.

Formerly known as Real-Time For the Masses.






# Drogue

https://www.drogue.io


https://book.drogue.io/


The Drogue IoT project aims to bring together data and users in an Internet of Things world.

Data comes from sensors and needs to travel a long way to reach its users. Of course, there is also the way back. All of these steps in the middle are still needed and must be managed in some way.

Drogue IoT provides you with tools, to create safe and efficient IoT solutions. Starting on the embedded side up to the cloud layer.






