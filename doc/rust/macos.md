# Mac targets
```shell
rustup target add aarch64-apple-ios aarch64-apple-ios-sim x86_64-apple-ios aarch64-apple-darwin x86_64-apple-darwin
```



# Cacao

This library provides safe Rust bindings for AppKit on macOS (beta quality, fairly usable) and UIKit on iOS/tvOS (alpha quality, see repo). It tries to do so in a way that, if you've done programming for the framework before (in Swift or Objective-C), will feel familiar. This is tricky in Rust due to the ownership model, but some creative coding and assumptions can get us pretty far.



# Lipo


https://crates.io/crates/cargo-lipo


```shell
cargo install cargo-lipo
```



# Rust xcode plugin

https://github.com/BrainiumLLC/rust-xcode-plugin

- working debug


# Matrix - rust bindings to ios version

https://github.com/matrix-org/matrix-rust-sdk/tree/main/bindings/apple




# mac-notification-sys

A simple wrapper to deliver or schedule macOS Notifications in Rust.


Example
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
    )
    .unwrap();

    send_notification(
        "NOW",
        None,
        "Without subtitle",
        Some(Notification::new().sound("Blow")),
    )
    .unwrap();
}
```

