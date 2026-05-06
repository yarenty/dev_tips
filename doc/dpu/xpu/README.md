---
title: "XPU (heterogeneous computing) — slide-deck collection"
keywords: [xpu, heterogeneous-computing, accelerators, dvpp, asset-folder]
status: reviewed
---

# XPU (heterogeneous computing) — slide-deck collection

This subsection is **a parking spot for reference slide decks**, not a set of articles. The XPU acronym (sometimes "X-PU", standing in for *any-PU* — CPU + GPU + NPU + DPU + DSP + custom ASICs all in one heterogeneous compute model) is used loosely in the literature, often in the context of Huawei Ascend / DaVinci, Tenstorrent, or vendor-neutral software-defined-acceleration discussions.

## What's actually in this folder

The slides below are kept as raw reference material — open them in PowerPoint / Keynote / [LibreOffice Impress](https://www.libreoffice.org/) for the original content. None of them have been translated into Markdown articles in this vault.

| File | Topic |
|------|-------|
| `2023 Software and hardware combination tech project Charter (English Version).pptx` | Project charter document, English translation |
| `Computing Acceleration Software and Hardware Integration Technology.pptx` | Overview of software/hardware co-design for acceleration |
| `DVPP.pptx` | DVPP (Digital Vision Pre-Processing) — Huawei Ascend's image / video pre-processing engine that sits in front of the AI compute units |

## Where to learn more

For DPUs / XPUs / heterogeneous compute generally:

- [Huawei Ascend / CANN](https://www.hiascend.com/en/) — the canonical XPU + DVPP stack ecosystem.
- [NVIDIA BlueField DPU](https://www.nvidia.com/en-us/networking/products/data-processing-unit/) — different angle: SmartNIC + ARM cores + custom accelerators.
- [Tenstorrent](https://tenstorrent.com/) — open-architecture AI accelerators (RISC-V + custom NoC).
- [AWS Nitro System](https://aws.amazon.com/ec2/nitro/) — purpose-built DPU+hypervisor offload for AWS.
- [[vitis]] — Xilinx/AMD's FPGA-based take on the same problem space.

## Cross-section see-also

- [[nvidia_omniverse]] — NVIDIA's full accelerator + simulation stack.
- [[groot]] — example of an embodied-AI workload these accelerators target.

## Keywords

`#dpu` `#xpu` `#heterogeneous-computing` `#accelerators` `#dvpp` `#huawei-ascend` `#asset-folder`
