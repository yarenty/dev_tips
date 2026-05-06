---
title: "Deskreen — share your screen to any device over WiFi"
main_link: https://github.com/pavlobu/deskreen
keywords: [deskreen, screen-sharing, wifi, electron, webrtc, second-monitor, airplay-alternative]
status: reviewed
---

# Deskreen — share your screen to any device over WiFi

**Main link:** <https://github.com/pavlobu/deskreen>

Site: <https://www.deskreen.com/>

## Summary

Deskreen is an open-source desktop app (Electron + WebRTC) that turns any device with a browser into a second screen for your computer. You start it on the host (macOS / Windows / Linux), scan a QR code on the client (phone, tablet, another laptop, smart TV), and a low-latency WebRTC stream of your screen — or just one application window — appears in the browser. Everything stays on your local network; nothing routes through a cloud service.

## Insight

This is the tool I reach for when AirPlay won't talk to a non-Apple device, Miracast won't talk to anything, and I don't want to install RustDesk just to mirror a window to my iPad for ten minutes. It's also handy as an **ad-hoc presenter view**: share a single window and use the iPad as a confidence monitor.

When to use it vs the alternatives:

- vs **AirPlay / Miracast** — Deskreen works cross-platform, no Apple/Microsoft hardware required, just a browser.
- vs **VNC / RDP / [[rustdesk]]** — Deskreen is one-way (mirror only, no remote control). Simpler to set up, less powerful.
- vs **Chromecast / Google Cast** — no Google account, no internet, runs over LAN only.
- For **screen extension** (treat the iPad as a real second monitor with mouse), look at Duet/Sidecar instead — Deskreen mirrors, it doesn't extend.

Caveats: Electron means a chunky binary, framerate depends on your LAN, and WebRTC sometimes needs a quick firewall poke on Windows.

## Similar / related topics

- [[rustdesk]] — open-source remote-desktop with full input control.
- [Sunshine](https://github.com/LizardByte/Sunshine) + [Moonlight](https://moonlight-stream.org/) — Nvidia GameStream-compatible self-hosted streaming for low-latency / gaming use cases.
- [Scrcpy](https://github.com/Genymobile/scrcpy) — same idea but Android-screen → desktop.
- [TigerVNC](https://tigervnc.org/) / [TightVNC](https://www.tightvnc.com/) — classic remote-frame-buffer protocol.
- AirPlay / Miracast — the OEM equivalents.

## Internal links

<!-- reviewed -->

- [[rustdesk]] — when you also need remote control, not just mirroring.
- [[weektodo]] — pair on a second screen for a permanent to-do view.
- [[superfile]] — TUI file-manager you might want mirrored to a side display.

## Keywords

`#deskreen` `#screen-sharing` `#wifi` `#webrtc` `#electron` `#second-monitor` `#design` `#tools`

## References / raw notes

> Deskreen — Share screen on same WiFi.
>
> <https://github.com/pavlobu/deskreen>

(Originally co-located in the now-removed `applications.md` stub alongside [[weektodo]] and [[rustdesk]] before the auto-split.)
