---
title: "Browsh: Firefox in your terminal"
main_link: https://www.brow.sh/
keywords: [browsh, terminal-browser, firefox, headless, tui, ssh, low-bandwidth, shell]
status: reviewed
review_date: 2026/05/03
---

# Browsh: Firefox in your terminal

**Main link:** <https://www.brow.sh/>

Repo: <https://github.com/browsh-org/browsh> · Docs: <https://www.brow.sh/docs/>

## Summary

Browsh is a text-based browser that drives a real headless Firefox in the
background and streams its rendered output to the terminal as a grid of
half-block characters and ANSI colour. JavaScript, CSS, web fonts, video
frames — everything Firefox can render, Browsh can show, just at the
resolution of your character cells. It can also run as a server you SSH into,
which is the original "browse the modern web over a 56k link" pitch.

## Insight

Browsh sits between [[lynx]] (no JS, instant, ASCII) and [[carbonyl]] (full
Chromium engine, mouse support, video). Its sweet spot is "I need a
JS-rendered page and I'm on a low-bandwidth or display-less link" — a phone
tethering, a remote server, an emergency SSH session. Page output is more
faithful to the real layout than Lynx, but interaction is awkward: clicking
small targets is fiddly, form input round-trips through Firefox, and you
still need a Firefox install (or the Docker image) on the host running it.

In practice I reach for [[lynx]] for quick reads, headless Chrome for
scripting, and Browsh only when I want to demo "Firefox over SSH" or when a
JS-only site refuses to render in anything else.

## Similar / related topics

- [[carbonyl]] — Chromium-in-terminal alternative; faster start, better video, worse text.
- [[lynx]] — text-only, no JS, ubiquitous; the right tool for static pages.
- [w3m](https://w3m.sourceforge.net/) — text browser with inline image support on supported terminals.
- [Firefox headless](https://wiki.mozilla.org/Firefox/Headless) — what Browsh wraps; useful directly for screenshots/PDFs.
- [[mosh]] — the latency-tolerant SSH that pairs well with Browsh-over-SSH.

## Internal links

- [[carbonyl]]
- [[lynx]]
- [[mosh]]
- [[terminals]]
- [[tools/shell/tools|shell tools]]

## Keywords

`#browsh` `#terminal-browser` `#firefox` `#tui` `#ssh` `#low-bandwidth` `#shell`

## References / raw notes

Try it via Docker without polluting the host:

```shell
docker run --rm -it browsh/browsh
```

Or via Homebrew on macOS / Linuxbrew (also requires Firefox on PATH):

```shell
brew install browsh
browsh
```

Run as an SSH-accessible server:

```shell
ssh brow.sh
```
