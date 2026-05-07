---
title: "Mosh — the mobile shell"
main_link: https://mosh.org/
keywords: [mosh, ssh, mobile, roaming, udp, predictive-echo, mit, remote]
status: reviewed
---

# Mosh — the mobile shell

**Main link:** <https://mosh.org/>

Repo: <https://github.com/mobile-shell/mosh>

## Summary

Mosh (the **mo**bile **sh**ell) is a remote terminal application that replaces SSH for interactive sessions over flaky or high-latency links. Born at MIT (Keith Winstein, Hari Balakrishnan, ~2012), it uses SSH only for initial authentication and then switches to its own UDP protocol (SSP) that supports IP roaming, intermittent connectivity, and **predictive local echo** — your keystrokes appear instantly even on a 500ms link, with corrections rolling in when the server replies. Licensed GPLv3.

## Insight

The killer scenarios:

- **Tethering or train Wi-Fi** that drops every few minutes — SSH gives you `Write failed: Broken pipe`, Mosh just resumes the moment the network comes back.
- **Roaming between networks** (work Wi-Fi → coffee shop → 5G) without re-authenticating — your IP can change, the Mosh session doesn't notice.
- **High-latency satellite / overseas links** — predictive local echo makes typing feel local even at 400ms RTT.
- **Laptop sleep** — close the lid, open it tomorrow, your shell session is still there.

Caveats:

- Mosh needs an **open UDP port range** on the server (default 60000–61000). On networks that block UDP this won't work — fall back to SSH + tmux.
- It does *not* multiplex; **always combine with [[tools/linux/tmux|tmux]] or [[zellij]]** so you get persistent sessions across server reboots and multiple windows.
- Server-side scrollback is lost on reconnect (Mosh is a synchronised-screen protocol, not a stream). Pair with a multiplexer's scrollback if you need it.
- No agent forwarding, no port forwarding. SSH still wins when you need those.

The canonical setup for a remote dev box is `mosh user@host -- tmux attach`.

## Similar / related topics

- [OpenSSH](https://www.openssh.com/) — the baseline; Mosh uses it for auth, then takes over.
- [[tools/linux/tmux|tmux]] / [[zellij]] — the multiplexer you should always pair Mosh with.
- [[byobu]] — Byobu over Mosh is a one-command "resilient remote dev environment".
- [Eternal Terminal (`et`)](https://eternalterminal.dev/) — TCP-based alternative with similar resume semantics.
- [[terminals]] — comparison of remote-terminal stacks (Byobu / Mosh / Xpra).

## Internal links

- [[tools/linux/tmux|tmux]]
- [[zellij]]
- [[byobu]]
- [[terminals]]

## Keywords

`#mosh` `#ssh` `#mobile` `#roaming` `#udp` `#predictive-echo` `#remote` `#linux`

## References / raw notes

### What it is

- A remote terminal application that supports stable connectivity across intermittent links by means of roaming and speculative echo/editing.
- Licensed GPLv3. Authored at MIT.
- Sources: <https://github.com/mobile-shell/mosh>, <https://mosh.org/>.

### Install

```shell
# macOS
brew install mosh

# Debian/Ubuntu
sudo apt install mosh

# Arch
sudo pacman -S mosh
```

### Server requirements

- `mosh-server` must be installed on the remote host.
- UDP ports 60000–61000 must be reachable (configurable via `--server="mosh-server new -p 60001"`).
- SSH is still required for the initial handshake.

### Typical usage

```shell
# basic
mosh user@host

# attach to a persistent tmux session on connect (recommended)
mosh user@host -- tmux new -A -s main

# pin to a specific UDP port (useful when firewall only opens one)
mosh --server="mosh-server new -p 60001" user@host
```

### Trade-offs

- Pros: roaming, instant local echo, survives sleep & network drop.
- Cons: needs UDP, no port forwarding, no agent forwarding, server-side scrollback not preserved.

### Follow-ups (open questions)

- How does Mosh manage prediction errors and latency on slow links? (Answer: SSP marks predicted text as underlined until the server confirms; mispredictions are silently corrected.)
- Where to find GPLv3 details: <https://www.gnu.org/licenses/gpl-3.0.html>.
