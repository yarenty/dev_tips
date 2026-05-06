---
title: "WireGuard — modern in-kernel VPN"
main_link: https://www.wireguard.com/
keywords: [wireguard, vpn, kernel, tunnel, networking, cryptography, tailscale, headscale]
status: reviewed
---

# WireGuard — modern in-kernel VPN

**Main link:** <https://www.wireguard.com/>

Quick-start: <https://www.wireguard.com/quickstart/> · `wg(8)` man page: <https://man7.org/linux/man-pages/man8/wg.8.html>

## Summary

WireGuard is a modern VPN protocol designed by Jason Donenfeld. It runs in the Linux kernel (since 5.6), uses a small fixed cryptographic suite (Curve25519, ChaCha20, Poly1305, BLAKE2s, SipHash24, HKDF), and is configured via short text files instead of certificates. The codebase is famously tiny — a few thousand lines, vs hundreds of thousands for OpenVPN/IPsec — which makes it auditable in a way the older protocols simply aren't.

## Insight

Reach for raw WireGuard when you want a **point-to-point encrypted tunnel** between machines you control: laptop ↔ home server, two cloud regions, a fleet of VPSes, an IoT device behind NAT and a public box. Configuration is two key pairs and four lines of TOML-ish config per peer; performance is "just kernel networking" fast.

For most humans, however, you should reach for one of the **managed control planes** built on top of WireGuard, not the raw protocol:

- **[Tailscale](https://tailscale.com/)** — WireGuard data plane + a hosted coordination server. NAT traversal, ACLs, MagicDNS, SSO, all just work. Free for personal use. The right answer for "give my devices a VPN" 90% of the time.
- **[Headscale](https://github.com/juanfont/headscale)** — open-source self-hosted reimplementation of the Tailscale control server. Same client app, your infrastructure.
- **[Netbird](https://netbird.io/)**, **[Innernet](https://github.com/tonarino/innernet)**, **[Nebula](https://github.com/slackhq/nebula)** — other mesh-VPN systems in the same niche (Nebula isn't WireGuard, but solves the same shape of problem).

Pick raw `wg` / `wg-quick` when:

- You want one tunnel and don't want a control plane.
- You're embedding it in something else (e.g., behind [[rathole]]-style port forwarding, or as the data plane of your own service).
- You're on a constrained device where the Tailscale daemon is too heavy.

vs OpenVPN / IPsec — WireGuard is dramatically simpler to set up, faster, easier to audit, and runs in-kernel. OpenVPN still wins on extreme NAT-traversal flexibility and richer auth integration; IPsec still wins on "the network team will only allow IPsec".

Gotchas:

- **No connection state on the wire** — `wg show` says a peer "exists" whether or not it's reachable. To tell if a tunnel is healthy, look at the last-handshake timestamp and the rx/tx counters.
- **Peers behind NAT** must send `PersistentKeepalive = 25` (or similar) or the NAT mapping will time out and inbound packets will silently drop.
- **Routing is your problem.** WireGuard sets up the interface and the crypto; getting traffic to actually use it requires correct `AllowedIPs`, route tables, and (for full-tunnel VPN) policy routing or `wg-quick`'s `Table = ` + fwmark trick.

## Similar / related topics

- [Tailscale](https://tailscale.com/) — managed mesh on top of WireGuard; the easy mode.
- [Headscale](https://github.com/juanfont/headscale) — self-hosted Tailscale-compatible control server.
- [OpenVPN](https://openvpn.net/) — the older, more flexible, more complex incumbent.
- [Nebula](https://github.com/slackhq/nebula) — Slack's mesh VPN; not WireGuard, but adjacent.
- [[rathole]] — port-forwarding alternative when you don't need a full L3 VPN.

## Internal links

<!-- reviewed -->

- [[rathole]]
- [[ngrok]]
- [[oracle_free_tier]]
- [[sniffnet]]

## Keywords

`#wireguard` `#vpn` `#kernel` `#tunnel` `#networking` `#cryptography` `#security` `#tools`

## References / raw notes

### Quick start

Read the [conceptual overview](https://www.wireguard.com/papers/wireguard.pdf) first, then install WireGuard. After that, the bring-up dance below.

### Side-by-side video

Two peers being configured simultaneously: <https://www.wireguard.com/talks/talk-demo-screencast.mp4>

A single configuration:

![wg walkthrough](https://www.wireguard.com/img/walkthrough.gif)

### Command-line bring-up

Add a new interface via `ip-link(8)` (auto-loads the module):

```shell
# ip link add dev wg0 type wireguard
```

(Non-Linux users will instead run `wireguard-go wg0`.)

Assign an IP and (optionally) a peer with `ip-address(8)`:

```shell
# ip address add dev wg0 192.168.2.1/24
```

For a point-to-point pair:

```shell
# ip address add dev wg0 192.168.2.1 peer 192.168.2.2
```

Configure keys and peer endpoints with `wg(8)`:

```shell
# wg setconf wg0 myconfig.conf
```

or imperatively:

```shell
# wg set wg0 listen-port 51820 \
    private-key /path/to/private-key \
    peer ABCDEF... \
    allowed-ips 192.168.88.0/24 \
    endpoint 209.202.254.14:8172
```

Bring the link up:

```shell
# ip link set up dev wg0
```

`wg show` and `wg showconf` display current state. `wg` with no args is `wg show` on every WireGuard interface.

![wg tool](https://www.wireguard.com/img/wg-tool.png)

Most of the bring-up dance is automated by `wg-quick(8)`:

![wg-quick tool](https://www.wireguard.com/img/wg-quick-tool.gif)

### Key generation

WireGuard uses base64 Curve25519 keys generated by `wg(8)`:

```shell
$ umask 077
$ wg genkey > privatekey
$ wg pubkey < privatekey > publickey

# or all at once
$ wg genkey | tee privatekey | wg pubkey > publickey
```

### NAT and firewall traversal

WireGuard is intentionally silent — it sends no packets when the application sends none. That's a problem when a peer sits behind NAT or a stateful firewall: the mapping times out and inbound packets get dropped. The fix is **persistent keepalives**: a tiny packet every N seconds to keep the NAT entry alive.

A sensible interval is 25 seconds. `0` disables it (the default). Set it via `PersistentKeepalive = 25` in the peer block of the config file or `persistent-keepalive` on the command line. Don't enable it if you don't need it — it adds chatter for no benefit on direct links.

### Demo server

For testing only:

```shell
$ sudo contrib/examples/ncat-client-server/client.sh
```

This brings up `wg0` over a deliberately insecure transport. You can then hit the hidden site or ping the gateway:

```shell
$ chromium http://192.168.4.1
$ ping 192.168.4.1
```

To route all internet traffic through the demo server:

```shell
$ sudo contrib/examples/ncat-client-server/client.sh default-route
$ curl zx2c4.com/ip
163.172.161.0
demo.wireguard.com
curl/7.49.1
```

The demo server is monitored — don't abuse it.

### Debugging

Linux kernel module with dynamic debug enabled:

```shell
# modprobe wireguard && echo module wireguard +p > /sys/kernel/debug/dynamic_debug/control
```

Userspace implementation: `export LOG_LEVEL=verbose`.
