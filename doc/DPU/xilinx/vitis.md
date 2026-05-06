---
title: "AMD Vitis (Xilinx Vitis): unified FPGA / DPU / accelerator software platform"
main_link: https://www.xilinx.com/products/design-tools/vitis.html
keywords: [vitis, xilinx, amd, fpga, dpu, vivado, vck5000, c1100, kv260, petalinux, install]
status: reviewed
---

# AMD Vitis (Xilinx Vitis): unified FPGA / DPU / accelerator software platform

**Main link:** <https://www.xilinx.com/products/design-tools/vitis.html>

Install troubleshooting (libtinfo5 on Ubuntu): <https://support.xilinx.com/s/article/76616?language=en_US>

## Summary

[Vitis](https://www.xilinx.com/products/design-tools/vitis.html) (now branded as **AMD Vitis** since AMD's 2022 Xilinx acquisition) is the unified software-and-IP platform for developing on Xilinx FPGAs and adaptive SoCs — Versal, Zynq UltraScale+, the Alveo accelerator cards (U50, U250, U280, VCK5000, C1100), and the Kria SoMs (KV260, K26). It bundles **Vivado** (the FPGA logic synthesis / place-and-route tool) plus high-level acceleration libraries, the Vitis HLS (High-Level Synthesis) C/C++/OpenCL→hardware compiler, the Vitis AI quantisation/compilation toolchain for the DPU (Deep-learning Processing Unit) IP, and the embedded-Linux toolchain (PetaLinux) for SoC targets.

In short: if you're writing software *that runs on a Xilinx accelerator*, Vitis is the IDE and toolchain you'll use.

## Insight

Three things to know before kicking off an install on Linux:

1. **Install `libtinfo5` first.** Modern Ubuntu/Debian/Mint dropped it in favour of `libtinfo6`, but the Vitis installer still calls into `libtinfo.so.5` and silently does the wrong thing if it's missing. The official errata at <https://support.xilinx.com/s/article/76616> documents this. `sudo apt install libtinfo5` before launching the GUI installer.
2. **Plan for ~150 GB and many hours of disk I/O.** A full Vitis install (Vitis + Vivado + HLS + Vitis AI + PetaLinux + simulator IPs) is genuinely huge. SSD essential; expect 4-8 hours on a midrange machine; install only the parts you'll use. **Mount `/tools` (or wherever you install) on a dedicated partition with journaling disabled** if you want to halve install time at the cost of replay durability — you can re-enable journaling after.
3. **Pick your install components by target board.** The full installer ticks everything by default; uncheck what you don't need:
   - **Alveo accelerator cards** (VCK5000, U50, U250, C1100) — pure host-PCIe accelerators, no embedded Linux. **No PetaLinux needed.**
   - **Kria SoMs** (KV260, K26) — an embedded SoC that boots a Linux distribution. **PetaLinux needed.**
   - For most users picking just **"Vitis (Unified Software Platform)"** in the top-level radio button is enough — it pulls Vivado as a transitive dependency.

The naming changes since 2022 are confusing if you're searching for tutorials:

| Era | What it was called | Now |
|-----|---------------------|-----|
| Pre-2019 | SDx / SDAccel / SDSoC / Vivado HLS | Folded into **Vitis** |
| 2019-2022 | Xilinx Vitis | Same product, rebranded |
| 2022+ | **AMD Vitis** | Same product, AMD branding after the acquisition |

The toolchain is the same; the docs and downloads have moved. Always check both `xilinx.com` and `amd.com/.../adaptive-computing` if a link 404s.

## Similar / related topics

- **Vitis AI** (`Vitis-AI` repo, <https://github.com/Xilinx/Vitis-AI>) — the ML-on-DPU subset of the toolchain. Quantises a TF/ONNX/PyTorch model and compiles it for the Xilinx DPU IP.
- **PetaLinux** — the embedded-Linux toolchain for Kria / Zynq UltraScale+ / Versal SoC targets. Yocto-based.
- **OpenCL on Vitis** — Vitis still supports OpenCL kernels for FPGAs, though most new dev has shifted to the more ergonomic Vitis HLS C/C++ flow.
- [Intel oneAPI / DPC++](https://www.intel.com/content/www/us/en/developer/tools/oneapi/overview.html) — the closest-shaped competitor for FPGA/CPU/GPU code from a single source. Different vendor ecosystem entirely.
- [[nvidia_omniverse]] — the NVIDIA side of the "accelerator + simulation" coin (different chip, different stack, but adjacent problem space).

## Internal links

<!-- reviewed -->

- [[nvidia_omniverse]] — NVIDIA's accelerator stack as comparison
- [[k3s]] — what you'd run on Kria SoMs once PetaLinux boots if you want a Kubernetes substrate

## Keywords

`#dpu` `#vitis` `#xilinx` `#amd` `#fpga` `#vivado` `#vitis-ai` `#vck5000` `#c1100` `#kv260` `#petalinux` `#install`

## References / raw notes

### Recipe: install Vitis on Ubuntu / Linux Mint

```sh
# Pre-flight: install the missing legacy ncurses lib
sudo apt update
sudo apt install libtinfo5

# Optional but highly recommended: dedicated partition for /tools
# with journaling disabled to halve install time
# (set up via parted / gparted before the install)

# Run the GUI installer (downloaded from amd.com/xilinx.com)
chmod +x ~/Downloads/Xilinx_Unified_*_Lin64.bin
~/Downloads/Xilinx_Unified_*_Lin64.bin

# In the GUI:
#   - Pick the TOP radio button: "Vitis (Unified Software Platform)"
#     (Vivado comes as a dependency)
#   - Uncheck device families you don't use (e.g. PetaLinux if you're
#     only targeting Alveo / VCK5000 / C1100)
#   - Install target: /tools/Xilinx (or /opt/Xilinx)
#
# Expect 4-8 hours. The author's note on the original Mint install:
# 8 hours on a SSD with ReiserFS + journaling enabled.
```

### Reference notes from the original install

> Just installed Vitis on Linux Mint.
>
> If you plan to install Vitis on any Ubuntu-based Linux, make sure to install `libtinfo5` before starting the installer. <https://support.xilinx.com/s/article/76616?language=en_US>
>
> I recommend mounting your `/tools` directory (or any other installation target) on a dedicated partition. This way, you can disable file-system journaling in the hope of speeding up the installation process. Mine took 8 hrs on a SSD drive using ReiserFS with journaling enabled.
>
> It is also recommended to uncheck all components and parts you don't use, especially if you use an accelerator card, i.e. VCK5000, C1100 (→ no PetaLinux). The KV260 runs an embedded processor booting Linux → PetaLinux needed.
>
> Only install Vitis (the uppermost radio button). Vivado comes with this.

### Useful entry points

- Product page: <https://www.amd.com/en/products/software/adaptive-socs-and-fpgas/vitis.html>
- Vitis docs: <https://docs.amd.com/r/en-US/ug1393-vitis-application-acceleration>
- Vitis AI repo (DL on DPU): <https://github.com/Xilinx/Vitis-AI>
- libtinfo5 install troubleshooting: <https://support.xilinx.com/s/article/76616?language=en_US>
- Forum (good for "I hit error X mid-install" searches): <https://support.xilinx.com/s/topic/0TO2E000000QS0HWAW/vitis>
