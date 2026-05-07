---
title: "mtr — traceroute + ping in one TUI"
main_link: https://github.com/traviscross/mtr
keywords: [mtr, traceroute, ping, network-diagnostics, latency, packet-loss, icmp, networking]
status: reviewed
---

# mtr — traceroute + ping in one TUI

**Main link:** <https://github.com/traviscross/mtr>

Project site: <https://www.bitwizard.nl/mtr/>

## Summary

`mtr` ("My Traceroute") combines `traceroute` and `ping` into one continuously-updating diagnostic. It first traces the route to a host, then keeps probing every hop with ICMP (or UDP/TCP) and updates a live table of loss %, last/avg/best/worst RTT, jitter, and standard deviation per hop. It's been around since the late 1990s and is in every distro's package manager.

## Insight

This is the tool to reach for the moment someone says "the connection feels slow" or "I'm getting timeouts to X". A single `mtr` run for 30 seconds tells you, much more reliably than `ping` alone, **which hop the problem starts at** — and whether it's loss or latency or both. Static `traceroute` runs miss intermittent loss; `ping` alone can't tell you whether the problem is your router, your ISP, the transit network, or the destination.

How to read it sanely:

- **Loss at hop N but not at hop N+1** is almost always not loss — it's an intermediate router rate-limiting ICMP TTL-exceeded responses to itself. Ignore it.
- **Loss starting at hop N and continuing to the end** is real and starts at hop N.
- **High RTT at one hop that doesn't propagate** is the same ICMP rate-limiting story.
- **Asymmetric routing** means the return path may be different from what you see — `mtr` from both ends if you can.

When `--report` mode (or `-r`) is more useful than the live UI: scripted runs, sharing output in a ticket, or alerting. `mtr -rwc 100 host` (report mode, wide output, 100 cycles) is a good "paste this into a bug" default.

Tooling alternatives:

- **[Trippy](https://github.com/fujiapple852/trippy)** ([[trace]]) — modern Rust TUI traceroute with a much richer interface (scrolling history, multiple destinations, IP info inline). Often the better default in 2024+.
- **`tracepath`** — minimal, no privileges needed, no real-time stats.
- **`hping3` / `nping`** — when you specifically need TCP/UDP probes against firewalled hosts.
- **`ping` alone** — fine when you already know the path and just want a single number.

Gotchas:

- ICMP-based by default, which many corporate firewalls drop. Use `--tcp` or `--udp` (`-T`/`-u`) and an appropriate port to get through.
- Needs raw sockets, so it either runs as root or via setuid / capability bits depending on distro.
- The hostname-resolution column (`-b` / `-z`) can be slow if reverse DNS is broken; toggle with `n` in the UI to flip to numeric.

## Similar / related topics

- [[trace]] — Trippy, the modern Rust TUI traceroute.
- [traceroute](https://man7.org/linux/man-pages/man8/traceroute.8.html) — the original; one-shot, less informative.
- [tracepath](https://man7.org/linux/man-pages/man8/tracepath.8.html) — unprivileged, minimal alternative.
- [[sniffnet]] — passive traffic monitor; pairs with `mtr` when you want to see what's actually flowing alongside the latency view.
- [[wireguard]] — useful to `mtr` over a tunnel to confirm the path actually goes where you think.

## Internal links

- [[trace]]
- [[sniffnet]]
- [[wireguard]]
- [[rustscan]]

## Keywords

`#mtr` `#traceroute` `#ping` `#network-diagnostics` `#latency` `#packet-loss` `#icmp` `#networking`

## References / raw notes

> mtr combines the functionality of the `traceroute` and `ping` programs in a single network diagnostic tool.
>
> As mtr starts, it investigates the network connection between the host mtr runs on and a user-specified destination host. After it determines the address of each network hop between the machines, it sends a sequence of ICMP ECHO requests to each one to determine the quality of the link to each machine. As it does this, it prints running statistics about each machine.

Source: <https://github.com/traviscross/mtr> · Project page: <https://www.bitwizard.nl/mtr/>

### Useful invocations

```shell
mtr example.com                 # interactive TUI
mtr -rwc 100 example.com        # report mode, wide, 100 cycles — paste into a ticket
mtr -T -P 443 example.com       # TCP probes to port 443 (firewall-friendly)
mtr -u -P 53 1.1.1.1            # UDP probes to port 53
mtr -n example.com              # numeric output (skip reverse DNS)
```
