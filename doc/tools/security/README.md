---
title: Security
keywords: [security, tunneling, vpn, port-scan, network-monitor, wireguard, ngrok]
status: reviewed
---

# Security

Tools for tunnelling, VPNs, scanning, traffic inspection, and a couple of related desktop-packaging utilities that historically landed in this directory.

## Articles

### Tunnelling (expose a local service to the internet)

- [[ngrok]] — hosted HTTPS tunnel to localhost; the classic webhook-development tool, now freemium.
- [[rathole]] — Rust reverse-proxy for NAT traversal; self-hosted alternative to ngrok / frp.

### VPN

- [[wireguard]] — modern in-kernel VPN protocol; the foundation under Tailscale, Headscale, Mullvad, etc.

### Discovery & monitoring

- [[rustscan]] — fast Rust port scanner that pipes results into nmap.
- [[sniffnet]] — friendly Rust GUI for live network traffic inspection (a "what's eating my bandwidth?" tool).

### Web → desktop (filed here for historical reasons)

- [[tools/security/pake|pake]] — Tauri-based wrapper that turns any webpage into a tiny desktop app. Not really security tooling, but this is where the article currently lives.

## Cross-section see-also

- [[mtr]] — `traceroute + ping` in one (under `tools/networking/`); pairs naturally with rustscan and sniffnet.
- [[trace]] — Trippy, a modern TUI traceroute.
- [[shodan]] — internet-wide scan engine; the offensive counterpart to running rustscan locally.
- [[oracle_free_tier]] — free always-on VM, useful as the public endpoint for a rathole or WireGuard tunnel.
- [[messaging/mqtt|mqtt]] — broker protocol commonly run behind one of these tunnels for IoT exposure.

## Keywords

`#security` `#tunneling` `#vpn` `#wireguard` `#ngrok` `#port-scan` `#network-monitor` `#tools`
