---
title: "macOS Gatekeeper / quarantine tricks"
main_link: https://support.apple.com/guide/mac-help/mh40616/mac
keywords: [macos, gatekeeper, quarantine, xattr, spctl, security, unsigned-apps, tips]
status: reviewed
---

# macOS Gatekeeper / quarantine tricks

**Main link:** <https://support.apple.com/guide/mac-help/mh40616/mac>

## Summary

A small grab-bag of recipes for getting around macOS's Gatekeeper / quarantine refusal to launch unsigned or non-notarized apps — the "*&lt;App&gt; cannot be opened because the developer cannot be verified*" dialog. Covers per-file `xattr` removal, the System Settings "Open Anyway" escape hatch, and the global `spctl` disable for power users.

## Insight

Gatekeeper exists for good reasons (it stops trojaned downloads from auto-running), so reach for these tricks **deliberately**, not reflexively. A typical decision tree:

1. **Trusted single app** (e.g. a known open-source binary you just compiled or downloaded from GitHub Releases) — strip the quarantine xattr on that one path. Cheapest and most surgical.
2. **You forgot to do step 1 and got the dialog** — use *System Settings → Privacy & Security → Open Anyway*. This whitelists the specific binary without weakening the system globally.
3. **You're doing serious dev work and the dialog is a constant friction** — `sudo spctl --master-disable` exposes the *Anywhere* option in Security settings. Re-enable it (`spctl --master-enable`) when you're done. Don't leave it off on a daily-driver machine.

Notes:

- On modern macOS (Sequoia / Sonoma) Apple has removed some right-click "Open" shortcuts; the System Settings flow is now the official path.
- Quarantine is just an extended attribute (`com.apple.quarantine`); it's set by *the downloader* (Safari, curl with `-O` from some sources, browsers via LaunchServices), not by the file itself.
- Homebrew casks already strip quarantine for you — these tricks are mostly for manually-downloaded `.app` bundles, DMGs, or random binaries.

## Similar / related topics

- [[ios]] — the other side of Apple's signing world (mandatory for App Store distribution).
- [[osxcross]] — when you cross-compile a macOS binary, this is the friction your users will hit.
- [Apple's "Safely open apps" guide](https://support.apple.com/guide/mac-help/mh40616/mac) — official doc.
- [`xattr(1)`](https://ss64.com/osx/xattr.html) — the underlying tool for inspecting/removing extended attributes.
- [`spctl(8)`](https://ss64.com/osx/spctl.html) — the Gatekeeper assessment policy CLI.

## Internal links

- [[ios]]
- [[osxcross]]
- [[utm]]

## Keywords

`#macos` `#gatekeeper` `#quarantine` `#xattr` `#spctl` `#security` `#unsigned-apps` `#tips`

## References / raw notes

### 1. Strip the quarantine attribute from a single app

After downloading, open Terminal and run:

```bash
sudo xattr -r -d com.apple.quarantine /Applications/Nuclear.app
```

`-r` recurses into the bundle; `-d` deletes the named attribute. Replace the path with whatever `.app` you're trying to launch.

### 2. Use the System Settings "Open Anyway" escape hatch

If the app is still blocked after a launch attempt:

1. Open **System Settings → Privacy & Security**.
2. Scroll down to the security section — there will be a message naming the blocked app.
3. Click **Open Anyway** next to it and confirm.

This adds a per-app exception without touching global Gatekeeper policy.

### 3. Globally disable Gatekeeper (power-user, temporary)

For persistent friction during development:

```bash
sudo spctl --master-disable
```

This exposes the *Anywhere* option under Privacy & Security. Re-enable when you're done:

```bash
sudo spctl --master-enable
```

Check current status with:

```bash
spctl --status
```
