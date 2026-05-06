---
title: "Network tracing tools (Trippy, traceroute, mtr)"
main_link: https://trippy.rs/
keywords: [trippy, traceroute, mtr, ping, network, tracing, rust, bgp, tui]
status: reviewed
---

# Network tracing tools (Trippy, traceroute, mtr)

**Main link:** <https://trippy.rs/>

Repo: <https://github.com/fujiapple852/trippy>

## Summary

Trippy is a Rust TUI that combines `traceroute` and `ping` into one continuous, interactive view of the path to a host: hop-by-hop latency, loss, AS / GeoIP info, and history graphs. Created by Andrew Forward (`fujiapple852`). It runs on Linux, BSD, macOS, and Windows. This article also covers the wider category — when to reach for `traceroute`, `mtr`, `tcpdump`, `strace`, `dtrace`, `bpftrace`, `eBPF` — i.e. tracing-flavoured tools beyond just network paths.

## Insight

For network-path debugging, the picker is roughly:

- **Quick "is the box reachable?"** — `ping`.
- **"Where does it break?" once** — classic `traceroute` (or `tracepath` if you don't have raw-socket privs).
- **Continuous "where is the loss happening?" while you watch** — [`mtr`](https://www.bitwizard.nl/mtr/) or **Trippy**. Trippy wins on UI: per-hop history graphs, multiple protocols (ICMP / UDP / TCP), and AS lookup baked in.
- **DNS specifically** — `dig` / `dog` / [[dns_toys]].
- **What's flowing on the wire** — `tcpdump` / `tshark` / Wireshark.

For tracing *system calls* and *kernel events* (different problem, same word):

- **A single process** — `strace` (Linux), `dtruss` (macOS), `truss` (BSD).
- **Production-safe whole-system** — eBPF-based: [`bpftrace`](https://bpftrace.org/), [`bcc-tools`](https://github.com/iovisor/bcc), [Inspektor Gadget](https://www.inspektor-gadget.io/).
- **macOS / Solaris**: DTrace (still alive on macOS via SIP-disabled boots).

Trippy needs elevated privileges by default (raw sockets); see its privileges guide to run unprivileged via Linux capabilities (`cap_net_raw`).

## Similar / related topics

- [`mtr`](https://www.bitwizard.nl/mtr/) — long-running ncurses combination of ping + traceroute; Trippy's spiritual ancestor.
- [`traceroute`](https://traceroute.sourceforge.net/) — the classic.
- [`tcpdump`](https://www.tcpdump.org/) / [Wireshark](https://www.wireshark.org/) — packet capture.
- [`bpftrace`](https://bpftrace.org/) — DTrace-style eBPF scripting on Linux.
- [[dns_toys]] — DNS-side troubleshooting toolkit.
- [[debugger]] — once you have a packet trace, you may also need a process debugger.

## Internal links

<!-- reviewed -->

- [[debugger]]
- [[mosh]]
- [[dns_toys]]
- [[tools/linux/tmux|tmux]]

## Keywords

`#trippy` `#traceroute` `#mtr` `#ping` `#network` `#tracing` `#rust` `#tui` `#linux`

## References / raw notes

### Trippy

<https://github.com/fujiapple852/trippy>

Trippy combines the functionality of `traceroute` and `ping` and is designed to assist with the analysis of networking issues.

![Trippy demo](https://raw.githubusercontent.com/fujiapple852/trippy/master/assets/0.12.0/demo.gif)

### Install

Trippy runs on Linux, BSD, macOS, and Windows. Install from your package manager, a precompiled binary, or source.

```shell
# Cargo
cargo install trippy --locked

# Homebrew (macOS, Linux)
brew install trippy

# Debian/Ubuntu (recent versions)
sudo apt install trippy

# Arch
sudo pacman -S trippy
```

See the [installation guide](https://trippy.rs/install/) for distro-specific details.

### Run

```shell
# basic trace
sudo trip example.com

# pick the protocol
sudo trip --protocol tcp --target-port 443 example.com

# JSON report mode (for scripts / CI)
sudo trip --mode json --report-cycles 5 example.com
```

To run Trippy without `sudo`, grant the binary `cap_net_raw` (Linux) — see Trippy's [privileges guide](https://trippy.rs/reference/privileges/).
