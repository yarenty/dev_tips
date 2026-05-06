---
title: "Lynx: the original text-mode web browser"
main_link: https://lynx.invisible-island.net/
keywords: [lynx, terminal-browser, text-browser, accessibility, scripting, ncurses, shell]
status: reviewed
---

# Lynx: the original text-mode web browser

**Main link:** <https://lynx.invisible-island.net/>

Repo / mirror: <https://github.com/ThomasDickey/lynx-snapshots> · Manual: <https://lynx.invisible-island.net/lynx_help/Lynx_users_guide.html>

## Summary

Lynx is the venerable cursor-driven, text-only web browser, originally
written in 1992 at the University of Kansas and maintained for decades by
Thomas Dickey. It speaks HTTP, HTTPS, FTP, gopher, and even WAIS, renders
HTML as a linear stream of text and links, and runs anywhere ncurses runs.
No JavaScript, no images, no CSS, no surprises.

## Insight

Lynx is still useful in 2024 for three things:

1. **"Is my page readable without JS?"** — load your site in Lynx; if the
   nav, the article, and the footer are all there, you've passed the
   accessibility/SEO/no-JS smoke test essentially for free.
2. **Scripted browsing in pipelines.** `lynx -dump` produces a clean text
   render with footnoted links — much friendlier for piping to grep/awk than
   raw HTML, and lighter than firing up [[carbonyl]] or [[browsh]].
3. **Reading docs over a slow / cramped SSH session.** It starts in
   milliseconds and uses kilobytes of memory.

Where it fails: anything modern that needs JavaScript to render content,
which is unfortunately most of the web. For those pages reach for [[browsh]]
or [[carbonyl]], or just `curl` plus a headless Chrome.

## Similar / related topics

- [[carbonyl]] — full Chromium engine in a terminal; the opposite end of the spectrum.
- [[browsh]] — Firefox-backed terminal browser; middle ground.
- [w3m](https://w3m.sourceforge.net/) — modern text browser with table support and inline images on supported terminals.
- [elinks](http://elinks.or.cz/) — Lynx fork with frames, tabs, and limited JS.
- `curl` + `pup` / `htmlq` — when you want to script HTML extraction instead of read it.

## Internal links

<!-- reviewed -->

- [[browsh]]
- [[carbonyl]]
- [[must_have]]
- [[tools/shell/tools|shell tools]]

## Keywords

`#lynx` `#terminal-browser` `#text-browser` `#accessibility` `#scripting` `#ncurses` `#shell`

## References / raw notes

Install:

```shell
# macOS
brew install lynx

# Debian / Ubuntu
sudo apt install lynx
```

Useful one-liners:

```shell
# Browse interactively
lynx https://example.com

# Dump a page as plain text with numbered link footnotes
lynx -dump https://example.com

# Strip the link list and just print the text
lynx -dump -nolist https://example.com

# Quick "does this site work without JS" check
lynx -dump https://your-site.example | less
```
