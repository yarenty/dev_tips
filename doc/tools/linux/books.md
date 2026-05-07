---
title: "Linux reading list"
main_link: https://0xax.gitbooks.io/linux-insides/content/
keywords: [linux, books, kernel, internals, learning, reading-list, gitbook]
status: reviewed
---

# Linux reading list

**Main link:** <https://0xax.gitbooks.io/linux-insides/content/>

## Summary

A small curated list of long-form material for going deeper on Linux internals. Currently anchored on *Linux Insides* by `0xAX` — a free, community-maintained book on GitBook that walks through the Linux kernel boot path, memory management, interrupts, syscalls, and other subsystems by reading the actual source.

## Insight

Reach for *Linux Insides* when a man-page or blog post isn't enough and you want to understand *why* the kernel does something, traced through real source files (with commit-pinned references). It is unfinished and uneven in places, but the boot/initialisation chapters are excellent and hard to find anywhere else for free. Pair it with the in-tree `Documentation/` and a checked-out kernel tree so you can jump to the symbols it discusses.

If you only want practical sysadmin knowledge, this is the wrong book — pick *The Linux Programming Interface* (Kerrisk) or *Linux System Programming* (Love) instead.

## Similar / related topics

- [Linux Insides](https://0xax.gitbooks.io/linux-insides/content/) — the resource itself.
- [The Linux Kernel documentation](https://www.kernel.org/doc/html/latest/) — canonical, exhaustive, terse.
- [LWN.net](https://lwn.net/) — the best running commentary on kernel development.
- [[debugger]] — once you've read about the kernel, Valgrind/gdb help you debug user-space code that talks to it.
- [[tools/linux/tmux|tmux]] — useful for keeping reading + a kernel tree + a build pane side-by-side.

## Internal links

- [[debugger]]
- [[tools/linux/tmux|tmux]]
- [[helix]]

## Keywords

`#linux` `#books` `#kernel` `#internals` `#reading-list` `#gitbook` `#learning`

## References / raw notes

### Linux Insides

<https://0xax.gitbooks.io/linux-insides/content/>

A free book on GitBook that walks through the Linux kernel by reading the source. Maintained by `0xAX` and contributors on GitHub.
