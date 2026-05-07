---
title: "Bunnix — Drew DeVault's 'Unix clone in about a month' project"
main_link: https://drewdevault.com/2024/05/24/2024-05-24-Bunnix.html
keywords: [bunnix, unix, kernel, hare, drew-devault, monolithic-kernel, hobby-os, ext4, doom]
status: reviewed
---

# Bunnix — Drew DeVault's "Unix clone in about a month" project

**Main link:** <https://drewdevault.com/2024/05/24/2024-05-24-Bunnix.html>

## Summary

Bunnix is a small Unix-like operating system Drew DeVault built between April 21 and May 24, 2024 — 27 actual working days — as a recreational programming project. It's a monolithic kernel for x86_64 written largely in [Hare](https://harelang.org/) (Drew's own systems language) with a couple of C dependencies (lwext4 for ext4, libvterm for the terminal). The userspace is mostly third-party Unix software (dash, tcc, less, vim 5.7, sbase, mandoc, lolcat, Doom, Colossal Cave Adventure) ported on top of a musl-derived libc. It boots from a CD-ROM ISO under qemu or on real hardware (legacy boot or EFI), but requires a PS/2 keyboard — there is no USB stack.

## Insight

This is one of the more thoughtful contemporary "Unix from scratch" reports out there, and worth reading for several reasons:

- **It's an honest field report**, not a tutorial. Drew is candid about what was hard, what he'd redo, and what cross-pollinated from his other OS work ([[helios]]).
- **It's a credible argument for Hare** as a kernel-hacking language. Bunnix is real-enough kernel code (40 syscalls, fork/exec/pipe/dup, signals, a virtual filesystem with /dev populated, ext4 read/write, a real terminal emulator) and Drew explicitly says he doesn't think he could have built it this fast in C.
- **The monolithic-vs-microkernel section** is the most interesting paragraph. Helios is a microkernel; Bunnix is monolithic. Drew built both, and the post compares them on filesystem caching, scheduling, driver placement, and signals. The summary: "putting together Bunnix in just 30 days is a feat which would not have been possible with a microkernel design. At the end of the day, monolithic kernels are just much simpler to implement."
- **The pain points he identifies** (filesystem layer needed multiple rewrites, libvterm is great but undocumented, signals are an inherently ugly Unix design, scheduler design dominates everything else) are exactly the lessons OS course material doesn't really teach you.

The honest reasons to care about this project:

- Motivational reading for "I want to learn OS internals."
- A concrete data point on what one experienced systems engineer can build in a month.
- Evidence that Hare is past the toy-language stage for kernel work.

The honest reasons not to:

- It's an art project, explicitly not maintained for production. Drew wrote: "for the most part it is an art project which is now more-or-less complete." Don't run it on hardware you care about.
- It's not a tutorial; you cannot learn how to build a kernel by reading it (though you can learn things by reading the code).
- No USB, no networking, no multi-user, single-CPU only.

## What's actually in there

Drivers: PCI (legacy), AHCI block, GPT/MBR partition tables, PS/2 keyboards, serial ports, CMOS clocks, framebuffers, ext4 + memfs.

Kernel features: virtual filesystem; populated `/dev` (block devices, `null`, `zero`, `full`, `/dev/kbd`, `/dev/fb0`, serial/video TTYs, controlling tty); roughly complete VT100 terminal emulator; passable termios; ~40 syscalls including `clock_gettime`, `poll`, `openat`, `fork`, `exec`, `pipe`, `dup`, `dup2`, `ioctl`. Single-user (could be made multi-user with a few more days of work).

Bootloaders: a multiboot-compatible legacy-boot loader (Hare) and an EFI loader (C). Both load the kernel as ELF plus initramfs.

Userspace: musl-derived libc; netbsd-curses; the third-party software list above.

## Try it

```sh
qemu-system-x86_64 -cdrom bunnix.iso -display sdl -serial stdio
```

ISO download is linked from the post. Tested on a ThinkPad X220 and a Starlabs Starbook Mk IV.

DOOM keybindings are weird: WASD to move, right-Shift to shoot, Space to open doors. Reboot to exit.

## Similar / related topics

- [[helios]] — Drew's other OS, the microkernel companion piece.
- [Hare language](https://harelang.org/) — the systems language Bunnix is mostly written in.
- [Redox OS](https://www.redox-os.org/) — most active contemporary "Unix-like in a memory-safe systems language" project.
- [seL4](https://sel4.systems/) — the formally-verified microkernel for the serious-research end of the spectrum.
- [xv6](https://pdos.csail.mit.edu/6.828/2024/xv6.html) — the canonical teaching Unix; complementary reading.

## Internal links

- [[helios]] — sibling project, microkernel design.
- [[programming/unix_systems/README|unix systems]] — parent section.
- [[assembly]] — pairs naturally; you'll read a lot of disassembly when working on a kernel.

## Keywords

`#bunnix` `#unix` `#kernel` `#hare` `#drew-devault` `#monolithic-kernel` `#hobby-os` `#ext4` `#programming`

## References / raw notes

Source post: <https://drewdevault.com/2024/05/24/2024-05-24-Bunnix.html>

Selected verbatim notes from the post:

> I needed a bit of a break from "real work" recently, so I started a new programming project that was low-stakes and purely recreational. On April 21st, I set out to see how much of a Unix-like operating system for x86_64 targets that I could put together in about a month. The result is Bunnix.

On Helios cross-pollination:

> Some of Bunnix's code stems from an earlier project, Helios. This includes portions of the kernel which are responsible for some relatively generic CPU setup (GDT, IDT, etc), and some drivers like AHCI were adapted for the Bunnix system. I admit that it would probably not have been possible to build Bunnix so quickly without prior experience through Helios.

On scheduler design:

> Both Helios and Bunnix are single-CPU systems, but unlike Helios, Bunnix allows context switching within the kernel — in fact, even preemptive task switching enters and exits via the kernel. This necessitates multiple kernel stacks and a different approach to task switching. However, the advantages are numerous, one of which being that implementing blocking operations like disk reads and pipe(2) are much simpler with wait queues.

On signals:

> Another source of frustration was signals, of course. Helios does not attempt to be a Unix and gets away without these, but to build a Unix, I needed to implement signals, big messy hack though they may be... I also finally learned how signals work from top to bottom, and boy is it ugly. I've always felt that this was one of the weakest points in the design of Unix and this project did nothing to disabuse me of that notion.

On filesystem caching:

> One thing I was surprised to learn a lot about is filesystems. Helios, as a microkernel, spreads the filesystem implementation across many drivers running in many separate processes. This works well enough, but one thing I discovered is that it's quite important to have caching in the filesystem layer, even if only to track living objects. When I revisit Helios, I will have a lot of work to do refactoring (or even rewriting) the filesystem code to this end.

On the monolithic-vs-microkernel verdict:

> I'm quite sure that putting together Bunnix in just 30 days is a feat which would not have been possible with a microkernel design. At the end of the day, monolithic kernels are just much simpler to implement. The advantages of a microkernel design are compelling, however — perhaps a better answer lies in a hybrid kernel.

Roadmap items Drew flagged for if/when work resumes: directory cache + better caching generally; ironing out ext4 bugs; procfs and top; mmap; more signals (e.g. SIGSEGV); multi-user; NVMe / IDE block devices; ATAPI / ISO 9660; Intel HD audio; network stack; Hare toolchain in the base system; self-hosting.
