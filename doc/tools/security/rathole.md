---
title: "rathole — Rust reverse-proxy for NAT traversal"
main_link: https://github.com/rapiz1/rathole
keywords: [rathole, tunnel, nat-traversal, reverse-proxy, rust, frp, self-hosted, vps]
status: reviewed
review_date: 2026/05/03
---

# rathole — Rust reverse-proxy for NAT traversal

**Main link:** <https://github.com/rapiz1/rathole>

## Summary

rathole is a small, secure, high-performance reverse proxy written in Rust. It runs as a pair: a server on a public-IP host and a client behind NAT, and it forwards inbound traffic on the server's ports back to services on the client. Same shape as ngrok or [frp](https://github.com/fatedier/frp), but self-hosted and one statically-linked binary on each end.

## Insight

Reach for rathole when you have a home server, a Raspberry Pi, or a Docker host behind CGNAT and want a stable public endpoint without paying ngrok or trusting a SaaS provider with your traffic. Rent the cheapest VPS you can find (or use [[oracle_free_tier]]), point a DNS record at it, and run `rathole` on both ends — you now own the tunnel.

Compared to alternatives:

- **vs [[ngrok]]** — rathole gives you stable URLs, no per-tunnel limits, and no third party in the path. It costs you a VPS and a config file. ngrok wins on zero-setup convenience and the request-inspector for webhook dev.
- **vs [frp](https://github.com/fatedier/frp)** — rathole is the explicit "frp but Rust, smaller, faster" rewrite. The author's benchmarks show lower memory and higher throughput; the feature surface is intentionally narrower (no built-in dashboard, fewer protocol plugins). If you need frp's full plugin zoo, stay on frp.
- **vs [[wireguard]] + nginx** — a WireGuard mesh plus a reverse proxy on the VPS gives you more flexibility (any L3 traffic, not just configured ports) at the cost of more moving parts. rathole is the lower-ceremony answer when you just need "expose port 8080".
- **vs [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)** — Cloudflare is free and handles TLS, but it routes through Cloudflare and ties you to their DNS/edge. rathole keeps everything on infrastructure you control.

Gotcha: TLS termination is on you. Either run rathole in TLS mode with a cert on the server, or terminate TLS at an nginx/Caddy in front and forward plaintext over the tunnel.

## Similar / related topics

- [[ngrok]] — hosted equivalent; trade self-hosting for convenience.
- [frp](https://github.com/fatedier/frp) — the older Go reverse-proxy rathole is benchmarked against.
- [[wireguard]] — lower-level alternative; full L3 VPN instead of port forwarding.
- [bore](https://github.com/ekzhang/bore) — even smaller Rust tunneller; minimal feature set.
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) — managed alternative if you're OK with Cloudflare in the path.

## Internal links

- [[ngrok]]
- [[wireguard]]
- [[oracle_free_tier]]
- [[sniffnet]]

## Keywords

`#rathole` `#tunnel` `#nat-traversal` `#reverse-proxy` `#rust` `#self-hosted` `#tools`

## References / raw notes

> A secure, stable and high-performance reverse proxy for NAT traversal, written in Rust.
>
> rathole, like frp and ngrok, can help to expose the service on the device behind the NAT to the Internet, via a server with a public IP.

Typical deployment is two TOML config files — one on the public server (`server.toml`) listing exposed ports and shared tokens per service, and one on the NAT'd client (`client.toml`) mapping each service to its local target. See the upstream README for current config schema (it has churned across versions).
