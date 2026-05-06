---
title: Xilinx (now AMD Adaptive Computing)
keywords: [xilinx, amd, fpga, dpu, vitis, vivado, alveo, versal, kria]
status: reviewed
---

# Xilinx (now AMD Adaptive Computing)

Notes on developing for Xilinx FPGAs and adaptive SoCs. Xilinx was acquired by AMD in 2022 and the official branding is now **AMD Adaptive Computing** — the products and toolchains are unchanged but the URLs and marketing names have moved. Always check both `xilinx.com` and `amd.com/.../adaptive-computing` if a link 404s.

The single article below covers Vitis (the unified software platform); the wider hardware/IP catalogue (Versal AI Edge, Alveo U250 / U280 / VCK5000, Kria SoMs, Zynq UltraScale+) is best learned from <https://www.amd.com/en/products/adaptive-socs-and-fpgas.html> directly.

## Articles

- [[vitis]] — AMD Vitis (was Xilinx Vitis): unified FPGA / DPU / accelerator software platform. Install recipe with the `libtinfo5` gotcha, component-picking guidance per board (Alveo vs Kria), branding-history table, and pointers to Vitis AI / PetaLinux / Vivado.

## Cross-section see-also

- [[nvidia_omniverse]] — the GPU-side equivalent of "vendor-tied accelerator + simulation stack".
- [[k3s]] — what to run on a Kria SoM (KV260) once it's booted PetaLinux.
- [[groot]] — example of an AI workload that benefits from accelerator hardware.

## Keywords

`#xilinx` `#amd` `#fpga` `#dpu` `#vitis` `#vivado` `#alveo` `#versal` `#kria`
