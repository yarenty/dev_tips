---
title: swww
main_link: https://github.com/LGFae/swww
keywords: [swww, rust, wayland, wallpaper, daemon, desktop-customisation]
status: reviewed
---

# swww

**Main link:** <https://github.com/LGFae/swww>

## Summary

`swww` (Sway-Wallpaper-Woes Workaround) is an animated wallpaper daemon for Wayland compositors, written in Rust. It runs as a background process and exposes a small CLI (`swww img`, `swww query`, `swww kill`) for changing wallpapers — including animated GIF / WebP / APNG — at runtime with smooth transitions, without restarting the compositor.

## Insight

Niche-but-loved tool in the Linux desktop-customisation crowd, especially among Sway / Hyprland / River users. Reach for it when (a) you're on Wayland, (b) `swaybg` doesn't cut it because you want animations or live changes, and (c) you don't want a full DE wallpaper manager. Compared to neighbours: `swaybg` (the Sway built-in) is static-only; `mpvpaper` uses mpv to render video as wallpaper but is heavier; `hyprpaper` is the Hyprland-native option for static images. swww's design point is "animated + IPC + cheap" — it sleeps when nothing is animating, so the cost-when-idle is essentially zero. On X11 you'd reach for `feh`, `nitrogen`, or `xwinwrap` instead; swww deliberately doesn't try to support X.

```shell
# typical use
swww init               # start the daemon
swww img wallpaper.gif  # set animated wallpaper
swww img --transition-type wipe ~/Pictures/photo.png
```

## Similar / related topics

- `swaybg` — static-image companion that ships with Sway.
- `hyprpaper` — Hyprland's first-party wallpaper daemon.
- `mpvpaper` — uses mpv to render video as wallpaper; heavier but more flexible.
- `feh` / `nitrogen` — the X11 equivalents.
- Hyprland / Sway / River — the Wayland compositors swww is most often paired with.

## Internal links

<!-- reviewed -->
- [[ui]]
- [[../README]]

## Keywords

`#swww` `#rust` `#wayland` `#wallpaper` `#daemon` `#linux-desktop`

## References / raw notes

- Repo: <https://github.com/LGFae/swww>

A solution to your Wayland Wallpaper Woes — efficient animated wallpaper daemon for Wayland, controllable at runtime via a CLI.
