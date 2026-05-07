---
title: "UTM — virtualizer for macOS (QEMU + Apple Hypervisor)"
main_link: https://mac.getutm.app/
keywords: [utm, virtualization, qemu, apple-silicon, arm64, hypervisor, vm, macos, linux, windows]
status: reviewed
---

# UTM — virtualizer for macOS (QEMU + Apple Hypervisor)

**Main link:** <https://mac.getutm.app/>

Repo: <https://github.com/utmapp/UTM> · App Store (paid, identical app, supports the project): <https://apps.apple.com/us/app/utm-virtual-machines/id1538878817>

## Summary

UTM is a free, open-source macOS-native frontend for **QEMU** that uses Apple's **Hypervisor** virtualization framework for near-native speed when guest and host architectures match (ARM64 guest on Apple Silicon, or x86_64 guest on an Intel Mac), and falls back to QEMU emulation for everything else (x86_64 on Apple Silicon, ARM64 on Intel, plus exotic targets like ARM32, MIPS, PPC, RISC-V). It ships as a regular `.app`, has a polished SwiftUI-style GUI, and supports a wide gallery of pre-built guest images.

## Insight

UTM's sweet spot is **"I want a Linux or Windows VM on my Mac without paying for Parallels"** — especially on Apple Silicon, where it's one of the few options that handles both native ARM64 guests (fast, via Hypervisor) and legacy x86 guests (slower, via emulation) in one app.

How it stacks up:

- **vs Parallels Desktop** — Parallels is more polished (better integration, automatic shared folders / clipboard, snappier graphics) but is a paid annual subscription. UTM is free and open source, but rougher around the edges.
- **vs VMware Fusion** — Fusion is now free for personal use and also competent on Apple Silicon, but UTM has a friendlier UX for spinning up arbitrary ISOs and a wider range of emulated CPUs.
- **vs VirtualBox** — VirtualBox finally has experimental Apple Silicon support but is far behind UTM there. On Intel Macs it's still a reasonable free option.
- **vs Lima / Colima** — for **Linux-on-Mac specifically**, [Lima](https://github.com/lima-vm/lima) (and its Docker-flavored wrapper [Colima](https://github.com/abiosoft/colima)) are usually a better fit than UTM. They're CLI-driven, headless, do automatic file sharing and port forwarding, and are designed specifically for dev workflows (Docker, Kubernetes, building Linux binaries on macOS). Reach for UTM when you actually want a desktop / GUI / non-Linux guest.
- **vs cloud VMs** — sometimes just renting an ARM Linux box is simpler than running one locally.

Practical notes:

- The **App Store version** is the same code as the free download but costs a few dollars to support the project — useful if you want auto-updates via the App Store.
- Performance is great for native-arch guests; **x86 emulation on Apple Silicon is functional but slow** — fine for one-off testing, painful for a daily-driver dev VM.
- Apple's EULA technically only permits virtualizing macOS-on-macOS on Apple-branded hardware, and only with Apple Silicon-on-Apple Silicon for current macOS versions. UTM supports macOS guests, but you're responsible for the licensing.

## Similar / related topics

- [[osxcross]] — the alternative when you want to *build* macOS binaries on Linux instead of *running* Linux on a Mac.
- [Lima](https://github.com/lima-vm/lima) — headless Linux VMs on macOS, dev-workflow-focused.
- [Colima](https://github.com/abiosoft/colima) — Lima preset that gives you a drop-in Docker / Kubernetes runtime on macOS.
- [Parallels Desktop](https://www.parallels.com/products/desktop/) — paid commercial competitor, better polish.
- [QEMU](https://www.qemu.org/) — the underlying emulator UTM wraps.

## Internal links

- [[osxcross]]
- [[ios]]
- [[osx_tricks]]

## Keywords

`#utm` `#virtualization` `#qemu` `#apple-silicon` `#arm64` `#hypervisor` `#vm` `#macos` `#linux-on-mac`

## References / raw notes

### Pitch (from getutm.app)

> Windows. Linux. Meet Apple Silicon.
>
> UTM employs Apple's Hypervisor virtualization framework to run ARM64 operating systems on Apple Silicon at near native speeds. On Intel Macs, x86/x64 operating systems can be virtualized. In addition, lower performance emulation is available to run x86/x64 on Apple Silicon as well as ARM64 on Intel.
>
> For developers and enthusiasts, there are dozens of other emulated processors as well including: ARM32, MIPS, PPC, and RISC-V. Your Mac can now truly run anything.

### Install

Two equivalent options:

- Free download: <https://mac.getutm.app/>
- Paid App Store build (same code, supports development): <https://apps.apple.com/us/app/utm-virtual-machines/id1538878817>

Or via Homebrew cask:

```bash
brew install --cask utm
```

### Pre-built guest gallery

UTM publishes ready-to-import VMs for common Linux distributions and Windows variants:

<https://mac.getutm.app/gallery/>
