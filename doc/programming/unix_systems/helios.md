---
title: "Helios — Drew DeVault's microkernel hobby OS"
main_link: https://sr.ht/~sircmpwn/helios/sources
keywords: [helios, microkernel, drew-devault, hare, message-passing, hobby-os, capabilities]
status: reviewed
review_date: 2026/05/03
---

# Helios — Drew DeVault's microkernel hobby OS

**Main link:** <https://sr.ht/~sircmpwn/helios/sources>

## Summary

Helios is Drew DeVault's longer-running microkernel hobby OS, the companion piece to his shorter Bunnix experiment ([[building_unix_system]]). It's deliberately Not-A-Unix: a message-passing microkernel design with capability-based IPC, where the filesystem and most "syscalls" are implemented as separate user-space processes rather than as code in the kernel. Like Bunnix, it's mostly written in [Hare](https://harelang.org/) with C glue. Sources live on SourceHut.

## Insight

Helios is interesting almost entirely *as a foil* to Bunnix. Drew built both, then wrote a public retrospective comparing them. The takeaways from his own writing are worth more than any summary I could offer:

- **Microkernel design dramatically increases the time-to-bootable-Unix.** Helios has been a multi-year project; Bunnix took 27 days. Drew's verdict: a Unix clone in 30 days "would not have been possible with a microkernel design."
- **Filesystem caching is hard in a microkernel.** When the filesystem is split across processes communicating by message passing, you have to think hard about where caches live and how invalidation works. In a monolithic kernel a single shared object cache is trivial.
- **Avoiding signals is a feature.** Helios doesn't implement Unix-style signals at all because it isn't trying to be a Unix. After implementing signals in Bunnix, Drew's view that signals are "one of the weakest points in the design of Unix" only hardened.
- **A hybrid kernel may be the right answer.** This is the speculative conclusion of the Bunnix retrospective: take the parts of microkernel design that are clean (driver isolation, capability IPC) and the parts of monolithic design that are pragmatic (in-kernel filesystem cache, simpler scheduling).

When to actually look at Helios:

- Reading Hare code in the wild beyond toy examples.
- Studying a small contemporary microkernel without the size of seL4 or Genode.
- As prerequisite reading before the Bunnix retrospective makes full sense.

When to look elsewhere:

- You want a microkernel you can actually deploy. Use [seL4](https://sel4.systems/), [Redox](https://www.redox-os.org/), or [Genode](https://genode.org/).
- You want documentation. Helios is a hobby project; the source is largely the documentation.

## Similar / related topics

- [[building_unix_system]] — Bunnix, the monolithic counter-experiment.
- [seL4](https://sel4.systems/) — formally-verified microkernel; the serious end of the spectrum.
- [Redox OS](https://www.redox-os.org/) — Rust microkernel, the actively-developed contemporary.
- [MINIX 3](https://www.minix3.org/) — the canonical teaching microkernel.
- [Genode framework](https://genode.org/) — long-running microkernel framework targeting multiple kernels.
- [Hare language](https://harelang.org/) — what Helios is mostly written in.

## Internal links

- [[building_unix_system]] — companion project; the Bunnix article quotes this one extensively.
- [[programming/unix_systems/README|unix systems]] — parent section.
- [[assembly]] — kernel work demands fluency reading low-level output.

## Keywords

`#helios` `#microkernel` `#drew-devault` `#hare` `#message-passing` `#hobby-os` `#capabilities` `#programming`

## References / raw notes

Sources: <https://sr.ht/~sircmpwn/helios/sources>

Most of what's substantively documented about Helios at the user-facing level lives inside Drew's Bunnix retrospective ([[building_unix_system]]) — particularly the comparison of the two designs on filesystems, scheduling, signals, and memory management. Worth reading both posts in order: Helios first (sources / blog mentions), then Bunnix.
