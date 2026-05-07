---
title: "Carbonyl: Chromium that runs in your terminal"
main_link: https://github.com/fathyb/carbonyl
keywords: [carbonyl, terminal-browser, chromium, fathyb, tui, ansi, video-in-terminal, shell]
status: reviewed
---

# Carbonyl: Chromium that runs in your terminal

**Main link:** <https://github.com/fathyb/carbonyl>

Repo: <https://github.com/fathyb/carbonyl> · Demo / blog post: <https://fathy.fr/carbonyl>

## Summary

Carbonyl is a real Chromium build by Fathy Boundjadj patched to render its
viewport into a terminal using ANSI graphics instead of a GPU surface. It runs
JavaScript, executes WebAssembly, plays video and audio, and handles mouse
input — all inside an xterm-compatible TTY. It starts in well under a second
because the renderer never touches a window-system, and it ships as a
self-contained binary or Docker image.

## Insight

Carbonyl is the most capable text-mode browser in existence by a wide margin:
unlike [[lynx]] or [[browsh]] it is _the actual web platform_, just with
characters as pixels. That makes it great for demos, screencasts of "look,
YouTube in tmux", or running a modern web app over SSH on a box with no
display server.

Honest gotcha: it's almost never the right tool for actual browsing. Reading
text is worse than [[lynx]], inspecting layout is worse than DevTools, and
embedding it into scripts is worse than `curl` plus a real headless Chrome.
The use cases boil down to (a) the wow factor, (b) graceful degradation when
all you have is a terminal, and (c) integration tests that need a full
Chromium but no X server. For everything else, headless Chromium with the
DevTools protocol is what you actually want.

## Similar / related topics

- [[browsh]] — Firefox-backed terminal browser; better text rendering, much heavier runtime.
- [[lynx]] — the venerable text-only browser; no JS, no images, instant.
- [Chromium headless](https://developer.chrome.com/docs/chromium/headless) — what you actually use for scraping and CI.
- [w3m](https://w3m.sourceforge.net/) — text browser that can render inline images via sixel/iTerm2 protocols.
- [[terminals]] — pick one that handles truecolour and mouse events cleanly, otherwise Carbonyl looks broken.

## Internal links

- [[browsh]]
- [[lynx]]
- [[terminals]]
- [[tools/shell/tools|shell tools]]
- [[must_have]]

## Keywords

`#carbonyl` `#terminal-browser` `#chromium` `#tui` `#ansi` `#shell` `#fathyb` `#video-in-terminal`

## References / raw notes

Quick try (Docker, no install):

```shell
docker run --rm -ti fathyb/carbonyl https://wikipedia.org
```

Or grab the prebuilt binary from the [releases page](https://github.com/fathyb/carbonyl/releases) and run:

```shell
carbonyl https://youtube.com
```

Use a terminal with truecolour + mouse support (kitty, WezTerm, iTerm2,
Alacritty) for anything resembling a usable experience.
