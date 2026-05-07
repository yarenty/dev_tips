---
title: "Unix systems (small from-scratch OS curiosities)"
keywords: [unix, kernel, hare, microkernel, monolithic-kernel, drew-devault, hobby-os]
status: reviewed
---

# Unix systems (small from-scratch OS curiosities)

A small, frankly idiosyncratic collection of "people building Unix-like (or Unix-adjacent) operating systems from scratch in 2024" projects, plus one item that's misfiled under this folder for historical reasons. **This is a curiosity collection, not a how-to.** If you actually want to build an OS, OSDev wiki + xv6 + *Operating Systems: Three Easy Pieces* is the standard path.

## What's here

- [[building_unix_system]] — Drew DeVault's **Bunnix**: a Unix-clone monolithic kernel built in about a month (April-May 2024), mostly in Hare with some C glue. Includes a working DOOM port.
- [[helios]] — Drew DeVault's **Helios**: an earlier, longer-running message-passing microkernel project (Not-A-Unix-On-Purpose). Bunnix borrowed a lot of low-level setup code from Helios.
- [[minio]] — **MinIO**: the S3-compatible object store. *Note:* this file is filed under `unix_systems/` for historical / misclassification reasons; MinIO is not a Unix system, it's a storage server. The article describes it correctly.

## Honest framing

The Bunnix and Helios articles are interesting because Drew DeVault writes blog posts that are unusually candid about what worked, what didn't, and what he'd do differently. They're not tutorials — they're field reports. Useful as motivation for learning OS internals, less useful as reference material.

If you want production OS work to follow instead, look at:

- [seL4](https://sel4.systems/) — formally-verified microkernel; serious research.
- [Redox OS](https://www.redox-os.org/) — Rust microkernel OS, the most active "Unix-like in a memory-safe language" project.
- [Asterinas](https://github.com/asterinas/asterinas) — Linux-ABI-compatible Rust kernel.
- [Genode](https://genode.org/) — long-running microkernel framework.
- [Theseus](https://www.theseus-os.com/) — research OS that pushes Rust safety properties into kernel design.

## See also

- [[assembly]] — sibling: low-level reading prerequisite for any of this.
- [[programming/cpp/README|C++]] — many older OS code lives in C/C++; ABI questions matter.
- [[programming/rust/README|Rust]] — the language most contemporary "from scratch" projects pick.
- [[programming/README|programming]] — parent.

## Keywords

`#unix` `#kernel` `#hare` `#microkernel` `#monolithic-kernel` `#hobby-os` `#programming`
