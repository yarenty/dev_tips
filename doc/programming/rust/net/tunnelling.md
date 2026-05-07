---
title: "bore — minimal TCP tunnel in Rust"
main_link: https://github.com/ekzhang/bore
keywords: [rust, bore, tunnelling, tcp, nat, ngrok-alternative, ekzhang]
status: reviewed
---

# bore — minimal TCP tunnel in Rust

**Main link:** <https://github.com/ekzhang/bore>

## Summary

[`bore`](https://github.com/ekzhang/bore) is a tiny self-hostable **TCP tunnel** in Rust by [Eric Zhang](https://www.ekzhang.com/), of [Hyperbeam](https://hyperbeam.com/) and Modal fame. ~2 KLOC, single static binary, MIT-licensed. It exposes a local TCP port to the public internet by reverse-tunnelling through a server you run, bypassing NAT/firewall. That's it — no HTTP-aware routing, no auth dashboard, no web UI, no built-in TLS. Just `bore local 8080 --to bore.pub`.

## Insight

Reach for bore when you want **what ngrok does, but self-hosted, and only at TCP**. The killer use cases:

- **Demo a local dev server** to a colleague / customer over the internet without deploying anywhere.
- **Webhook reception** while developing locally (Stripe / GitHub / Twilio callbacks).
- **Expose a Raspberry Pi or home-lab service** without setting up a VPN or punching real holes in the router.
- **Bypass corporate / hotel / mobile NAT** to reach a service running on your laptop.

How it slots vs. neighbours:

| Tool | Shape | Self-host? | TLS? | HTTP-aware? | Auth? |
|------|-------|------------|------|-------------|-------|
| **bore** | TCP only | ✅ trivial | ❌ (BYO) | ❌ | optional shared secret |
| [ngrok](https://ngrok.com/) | TCP / HTTP / TLS | ❌ (SaaS) | ✅ | ✅ | account-based |
| [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) | HTTP(S) | ❌ (SaaS) | ✅ | ✅ | account |
| [frp](https://github.com/fatedier/frp) | TCP / UDP / HTTP / HTTPS / KCP / QUIC | ✅ | ✅ | ✅ | yes |
| [rathole](https://github.com/rapiz1/rathole) | TCP / UDP | ✅ (Rust) | ✅ via TLS | ❌ | yes |
| [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) | HTTPS | ❌ (SaaS) | ✅ | ✅ | tailnet |
| [chisel](https://github.com/jpillora/chisel) | TCP / SOCKS over HTTP | ✅ | ✅ | tunnel-only | yes |

bore's **deliberate minimalism** is its pitch: if you want HTTP routing, custom domains, request inspection, or audit logs, you've outgrown bore — go to ngrok / Cloudflare Tunnel / frp. If you want a 5 MB static binary that does one thing, bore wins.

For the *Rust* alternatives in the same space, bore and [`rathole`](https://github.com/rapiz1/rathole) are the two go-tos; rathole has more features (TLS, NoiseProtocol, multiple services, hot-reload config) at the cost of more knobs.

Gotchas:

1. **No TLS.** TCP-only means TLS termination is upstream's problem. Run nginx / Caddy in front if you need HTTPS.
2. **The public bore.pub server is best-effort.** Fine for demos, do not depend on it. Run your own with `bore server`.
3. **Random ports by default.** Each connection picks a random remote port; pin one with `--port 1234` if you want stability.

## Similar / related topics

- [`rathole`](https://github.com/rapiz1/rathole) — Rust frp-replacement; more features (TLS, multiple services) at the cost of more config.
- [ngrok](https://ngrok.com/) — the SaaS reference; HTTP-aware, has dashboards, custom domains, auth.
- [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) — HTTPS-only, free for personal use, ties into Cloudflare's edge.
- [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) — exposes a tailnet service to the public internet over HTTPS.
- [WireGuard](https://www.wireguard.com/) — different problem (full-tunnel VPN), but often the right answer instead of a reverse tunnel.

## Internal links

<!-- reviewed -->

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[tools/networking/ngrok|ngrok]] — the SaaS comparison point.
- [[tools/networking/rathole|rathole]] — the more-featured Rust alternative.
- [[tools/security/wireguard|WireGuard]] — the VPN cousin in the same problem-space.

## Keywords

`#rust` `#bore` `#tunnelling` `#tcp` `#nat` `#ngrok-alternative` `#self-hosted`

## References / raw notes

### bore

<https://github.com/ekzhang/bore>

> A modern, simple TCP tunnel in Rust that exposes local ports to a remote server, bypassing standard NAT connection firewalls. That's all it does: no more, and no less.

### Quick recipe

```sh
# install
cargo install bore-cli

# expose local port 8080 via the public test server
bore local 8080 --to bore.pub

# self-host the relay
bore server                    # listens on 0.0.0.0:7835 by default
bore local 8080 --to YOUR_HOST # from another machine
```
