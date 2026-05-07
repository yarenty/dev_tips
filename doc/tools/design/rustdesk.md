---
title: "RustDesk — open-source remote desktop"
main_link: https://rustdesk.com/
keywords: [rustdesk, remote-desktop, teamviewer-alternative, rust, self-hosted, cross-platform]
status: reviewed
review_date: 2026/05/03
---

# RustDesk — open-source remote desktop

**Main link:** <https://rustdesk.com/>

Repo: <https://github.com/rustdesk/rustdesk>

## Summary

RustDesk is a Rust-based, open-source remote-desktop application that aims to be a drop-in replacement for TeamViewer / AnyDesk. It supports Windows, macOS, Linux, Android, iOS, and the web — full input control (not just mirroring), file transfer, clipboard sync, and session recording. You can use the public relay servers or **self-host the relay (`hbbs`) and rendezvous (`hbbr`) servers** for full control.

## Insight

The use case that finally made me install it: a Mac at home and a Windows box at the office, no VPN, no Apple Remote Desktop license, no patience for TeamViewer's "we suspect commercial use" nag screens. RustDesk just worked.

When to reach for it:

- You need **bidirectional input control** — share AND drive a remote machine. (Use [[deskreen]] if you only need view-only mirroring.)
- You want to **self-host** the relay so no session metadata leaves your network.
- You hate TeamViewer's pricing or AnyDesk's UX and want an open-source escape hatch.
- You're supporting a non-technical family member and need cross-platform that "just works".

Comparisons:

- vs **TeamViewer** — RustDesk is free, open-source, no licensing nag.
- vs **AnyDesk** — similar latency, RustDesk wins on price (free) and openness.
- vs **VNC** — RustDesk has NAT traversal, encryption, and modern codecs out of the box.
- vs **Sunshine + Moonlight** — Sunshine is tuned for low-latency game streaming; RustDesk is tuned for desktop work.
- vs **[[deskreen]]** — Deskreen is one-way mirror; RustDesk is full remote control.

Caveats: the default public relay can be slow at peak times — self-host or buy a small VPS for the relay if you use it daily. Some Linux distros need a couple of clicks to grant screen-recording / accessibility permissions before it works.

## Similar / related topics

- [TeamViewer](https://www.teamviewer.com/) — the proprietary incumbent.
- [AnyDesk](https://anydesk.com/) — the proprietary challenger.
- [Sunshine](https://github.com/LizardByte/Sunshine) + [Moonlight](https://moonlight-stream.org/) — low-latency game-streaming pair.
- [Parsec](https://parsec.app/) — closed-source low-latency desktop streaming.
- [[deskreen]] — view-only screen-mirroring sibling.

## Internal links

- [[deskreen]] — when you only need to mirror, not control.
- [[wireguard]] — pair with WireGuard for end-to-end encrypted access without exposing the relay.
- [[rathole]] — alternative tunneling tool to expose a self-hosted relay.

## Keywords

`#rustdesk` `#remote-desktop` `#teamviewer-alternative` `#anydesk-alternative` `#rust` `#self-hosted` `#cross-platform` `#design` `#tools`

## References / raw notes

> Finally, connection between mac and win!
>
> <https://rustdesk.com/>
