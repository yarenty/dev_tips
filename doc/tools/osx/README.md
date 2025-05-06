# OSX

This directory contains notes, tips, and tricks for macOS and iOS development and usage.

## Contents

-   [OSX](osx.md) - General OSX information.
-   [iOS](ios.md) - iOS development and tricks.
-   [Slint GUI](../../programming/rust/gui/slint.mdogramming/rust/gui/slint.md) - Slint GUI for Rust.
-   [OSX Cross Compilation](osxcross.md) - Cross-compiling for macOS.
-   [OSX Tricks](osx_tricks.md) - macOS-specific tips.

> "Simplicity is the ultimate sophistication." - Leonardo da Vinci

 
## OSX Tricks
 macOS continually verifies new builds which can add a few seconds to your iteration cycles.

To fix this, you can:

Run 
```bash 
sudo spctl developer-mode enable-terminal
 ```
to enable the Developer Tools panel in System Settings.

In System Settings, search for "Developer Tools" and add your terminal (e.g. iTerm or Ghostty) to the list under "Allow applications to use developer tools"

Restart your terminal.

Thanks to the nextest developers for publishing [this](https://nexte.st/docs/installation/macos/#gatekeeper).