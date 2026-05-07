---
title: "XPU (heterogeneous computing)"
keywords: [xpu, heterogeneous-computing, accelerators, dvpp, huawei-ascend, tenstorrent]
status: reviewed
review_date: 2026/05/03
---

# XPU (heterogeneous computing)

The **XPU** acronym (sometimes "X-PU", standing in for *any-PU* — CPU + GPU + NPU + DPU + DSP + custom ASICs all in one heterogeneous compute model) is used loosely in the literature, often in the context of Huawei Ascend / DaVinci, Tenstorrent, or vendor-neutral software-defined-acceleration discussions.

## Where to learn more

For DPUs / XPUs / heterogeneous compute generally:

- [Huawei Ascend / CANN](https://www.hiascend.com/en/) — the canonical XPU stack, including DVPP (Digital Vision Pre-Processing) for image/video pipelines that sit in front of the AI compute units.
- [NVIDIA BlueField DPU](https://www.nvidia.com/en-us/networking/products/data-processing-unit/) — different angle: SmartNIC + ARM cores + custom accelerators.
- [Tenstorrent](https://tenstorrent.com/) — open-architecture AI accelerators (RISC-V + custom NoC).
- [AWS Nitro System](https://aws.amazon.com/ec2/nitro/) — purpose-built DPU + hypervisor offload for AWS.
- [[vitis]] — Xilinx/AMD's FPGA-based take on the same problem space.

## Cross-section see-also

- [[nvidia_omniverse]] — NVIDIA's full accelerator + simulation stack.
- [[groot]] — example of an embodied-AI workload these accelerators target.

## Keywords

`#dpu` `#xpu` `#heterogeneous-computing` `#accelerators` `#dvpp` `#huawei-ascend` `#tenstorrent`
