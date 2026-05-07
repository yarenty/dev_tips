---
title: mac-notification-sys
main_link: https://github.com/h4llow3En/mac-notification-sys
keywords: [mac-notification-sys, rust, macos, notifications, nsusernotification]
status: reviewed
---

# mac-notification-sys

**Main link:** <https://github.com/h4llow3En/mac-notification-sys>

## Summary

`mac-notification-sys` is a small one-purpose crate: it wraps macOS's `NSUserNotification` API so that a Rust program can deliver or schedule native banners/alerts. Maintained as part of the same author's `notify-rust` ecosystem, it's the macOS-specific backend that `notify-rust` shells out to.

## Insight

Reach for this crate when you only target macOS and want a tiny, zero-fluff dependency for "pop a notification". For cross-platform code use the umbrella [`notify-rust`](https://github.com/hoodie/notify-rust) crate instead — it abstracts over `mac-notification-sys` (macOS), the FreeDesktop `org.freedesktop.Notifications` D-Bus API (Linux), and Win32 toast on Windows. Two macOS gotchas worth knowing: (1) you must call `set_application` with a *registered bundle identifier* (e.g. Firefox's, Terminal's) or notifications silently no-op; (2) `NSUserNotification` itself was deprecated in macOS 11 in favour of `UNUserNotificationCenter`, which requires a real `.app` bundle to authorise — so unbundled CLI tools may eventually need a different path.

```rust
use mac_notification_sys::*;

fn main() {
    let bundle = get_bundle_identifier_or_default("firefox");
    set_application(&bundle).unwrap();

    send_notification(
        "Danger",
        Some("Will Robinson"),
        "Run away as fast as you can",
        None,
    ).unwrap();

    send_notification(
        "NOW",
        None,
        "Without subtitle",
        Some(Notification::new().sound("Blow")),
    ).unwrap();
}
```

## Similar / related topics

- `notify-rust` — cross-platform wrapper that uses this crate on macOS.
- `UNUserNotificationCenter` — the modern Apple API; needs a bundled `.app`.
- [[cacao]] — broader AppKit/UIKit bindings if you need more than notifications.
- `tauri-plugin-notification` — Tauri's notification plugin (uses similar OS APIs).

## Internal links

<!-- reviewed -->
- [[macos]]
- [[cacao]]
- [[../README]]

## Keywords

`#mac-notification-sys` `#rust` `#macos` `#notifications` `#nsusernotification`

## References / raw notes

- Repo: <https://github.com/h4llow3En/mac-notification-sys>
- Crate: <https://crates.io/crates/mac-notification-sys>
- Cross-platform companion: <https://github.com/hoodie/notify-rust>

A simple wrapper to deliver or schedule macOS notifications in Rust.
