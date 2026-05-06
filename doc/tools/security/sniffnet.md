---
title: "Sniffnet — friendly network traffic monitor"
main_link: https://github.com/GyulyVGC/sniffnet
keywords: [sniffnet, network-monitor, packet-capture, rust, gui, bandwidth, wireshark, tcpdump]
status: reviewed
---

# Sniffnet — friendly network traffic monitor

**Main link:** <https://github.com/GyulyVGC/sniffnet>

Site: <https://sniffnet.net/>

## Summary

Sniffnet is a cross-platform desktop GUI written in Rust by Giuliano Bellini that lets you watch the traffic going through one of your network interfaces in real time. You pick an adapter, optionally apply a filter, and you immediately get throughput charts, per-connection breakdowns, country/AS lookups, and notifications. It runs on Linux, macOS, and Windows.

## Insight

Sniffnet is the answer to "something is hammering my upload — what is it?" without having to remember `tcpdump` syntax or boot Wireshark for a five-minute investigation. It sits in the gap between "the OS network indicator is too vague" and "Wireshark is too much".

When to reach for it:

- A laptop suddenly sending gigabytes — figure out which process / remote host within 30 seconds.
- Demoing what "network traffic" looks like to someone who's never seen a packet capture.
- Spot-checking that a tunnel ([[ngrok]], [[rathole]], [[wireguard]]) is actually carrying the traffic you expect.
- Catching a chatty application (telemetry, sync clients) on an otherwise idle box.

Honest limits — this is **not** a Wireshark replacement:

- No deep packet-by-packet inspection / dissection.
- No PCAP-style display filters or follow-stream.
- Limited export — fine for "see what's happening now", less suited for forensic post-mortem.
- Needs raw-socket capability: on Linux that's typically `setcap cap_net_raw,cap_net_admin=eip` on the binary; on macOS/Windows you'll get a permission prompt or installer hook.

For deep dissection, switch to Wireshark or `tcpdump`/`tshark`. For long-term throughput dashboards, look at vnStat or a [[prometheus]] node-exporter feeding [[observability/grafana|grafana]].

## Similar / related topics

- [Wireshark](https://www.wireshark.org/) — the heavyweight; deep dissection of every packet.
- [tcpdump](https://www.tcpdump.org/) — terminal-only capture; what scripts use under the hood.
- [bandwhich](https://github.com/imsnif/bandwhich) — TUI alternative; per-process bandwidth in the terminal.
- [iftop](https://www.ex-parrot.com/pdw/iftop/) / [nethogs](https://github.com/raboof/nethogs) — older curses tools in the same niche.
- [[mtr]] — traceroute + ping; pairs with sniffnet when chasing a slow remote host.

## Internal links

<!-- reviewed -->

- [[mtr]]
- [[rustscan]]
- [[wireguard]]
- [[ngrok]]

## Keywords

`#sniffnet` `#network-monitor` `#packet-capture` `#rust` `#gui` `#bandwidth` `#tools` `#security`

## References / raw notes

> Application to comfortably monitor your Internet traffic.
> Cross-platform, Intuitive, Reliable.

Repo: <https://github.com/GyulyVGC/sniffnet>
