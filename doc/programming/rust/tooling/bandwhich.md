---
title: bandwhich — bandwidth-by-process terminal top
main_link: https://github.com/imsnif/bandwhich
keywords: [bandwhich, rust, network, bandwidth, top, sniff, tui, cli]
status: reviewed
review_date: 2026/05/03
---

# bandwhich — bandwidth-by-process terminal top

**Main link:** <https://github.com/imsnif/bandwhich>

## Summary

`bandwhich` is a CLI by Aram Drevekenin (also of [[zellij]]) that displays current network utilisation by process, connection, and remote host. It sniffs a network interface (libpcap), then cross-references each packet against `/proc` (Linux), `lsof` (macOS), or the Windows API to attribute bytes to the originating process. Reverse-DNS resolution runs in the background on a best-effort basis. It needs root / `cap_net_raw` to sniff the interface.

## Insight

Reach for `bandwhich` when *something on this machine is using bandwidth* and you need to know what. The killer feature is the **per-process attribution** — `iftop` shows you which IPs are talking, `nethogs` shows you per-process totals on Linux only, but `bandwhich` shows you both, on every OS, in one TUI. Common invocations:

```sh
sudo bandwhich                       # default; auto-pick interface
sudo bandwhich -i en0                # specific interface
sudo bandwhich --raw                 # plain log output (no TUI)
sudo bandwhich -n                    # no DNS resolution (faster)
```

**Gotchas**: needs root (or `setcap cap_net_raw,cap_net_admin+ep $(which bandwhich)` on Linux); WireGuard and VPN tunnels show up as the tunnel interface, not the underlying physical link; Wi-Fi monitor mode is not supported (this is not Wireshark). For pcap-style packet capture and protocol inspection, use [[../../../tools/security/sniffnet|sniffnet]] (Rust) or Wireshark.

Compared to siblings: **`iftop`** shows IP-level traffic only (no process); **`nethogs`** is the closest direct competitor but Linux-only and TUI-spartan; **`bmon`** focuses on per-interface bandwidth charts; **`nload`** is a per-interface gauge.

## Similar / related topics

- `iftop` — IP-level bandwidth top, no process attribution.
- `nethogs` — per-process net top, Linux-only.
- `bmon` / `nload` — per-interface bandwidth gauges.
- [[../../../tools/security/sniffnet|sniffnet]] — friendlier-than-Wireshark Rust packet inspector.
- [[bottom]] — general system top; includes a network panel but no per-process attribution.

## Internal links

- [[README]] — tooling section landing.
- [[bottom]] — general top; bandwhich complements its network panel.
- [[../../../tools/security/sniffnet|sniffnet]] — for deeper per-packet inspection.
- [[../../../tools/security/wireguard|wireguard]] — VPN-traffic gotcha source.
- [[../../../observability/node_exporter|node_exporter]] — for long-term per-host network metrics.

## Keywords

`#bandwhich` `#rust` `#network` `#bandwidth` `#top` `#tui` `#cli` `#sniff`

## References / raw notes

- Repo: <https://github.com/imsnif/bandwhich>
- Crate: <https://crates.io/crates/bandwhich>
- Sniffs the chosen interface, attributes bytes to processes via `/proc` (Linux), `lsof` (macOS), or WinAPI; resolves IPs to hostnames in the background.
- Install: `cargo install bandwhich` or via Homebrew/apt.
