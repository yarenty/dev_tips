
# What’s a DPU?

Specialists in moving data in data centers, DPUs, or data processing units, are a new class of programmable processor and will join CPUs and GPUs as one of the three pillars of computing.

Of course, you’re probably already familiar with the central processing unit. Flexible and responsive, for many years CPUs were the sole programmable element in most computers.

More recently the GPU, or graphics processing unit, has taken a central role. Originally used to deliver rich, real-time graphics, their parallel processing capabilities make them ideal for accelerated computing tasks of all kinds. Thanks to these capabilities, GPUs are essential to artificial intelligence, deep learning and big data analytics applications.

Over the past decade, however, computing has broken out of the boxy confines of PCs and servers — with CPUs and GPUs powering sprawling new hyperscale data centers.

These data centers are knit together with a powerful new category of processors. The DPU has become the third member of the data-centric accelerated computing model.

“This is going to represent one of the three major pillars of computing going forward,” NVIDIA CEO Jensen Huang said during a talk earlier this month.

“The CPU is for general-purpose computing, the GPU is for accelerated computing, and the DPU, which moves data around the data center, does data processing.”

# What's a DPU?
System on a chip that combines:
- Industry-standard, high-performance, software-programmable multi-core CPU
- High-performance network interface
- Flexible and programmable acceleration engines

# CPU v GPU v DPU: What Makes a DPU Different?
A DPU is a new class of programmable processor that combines three key elements. A DPU is a system on a chip, or SoC, that combines:

- An industry-standard, high-performance, software-programmable, multi-core CPU, typically based on the widely used Arm architecture, tightly coupled to the other SoC components.
- A high-performance network interface capable of parsing, processing and efficiently transferring data at line rate, or the speed of the rest of the network, to GPUs and CPUs.
- A rich set of flexible and programmable acceleration engines that offload and improve applications performance for AI and machine learning, zero-trust security, telecommunications and storage, among others.

- All these DPU capabilities are critical to enable an isolated, bare-metal, cloud-native computing platform that will define the next generation of cloud-scale computing.


![youtube intro](https://youtu.be/bOf2S7OzFEg)


# DPUs Incorporated into SmartNICs

The DPU can be used as a stand-alone embedded processor. But it’s more often incorporated into a SmartNIC, a network interface controller used as a critical component in a next-generation server.

Other devices that claim to be DPUs miss significant elements of these three critical capabilities.

DPUs, or data processing units, can be used as a stand-alone embedded processor, but they’re more often incorporated into a SmartNIC, a network interface controller that’s used as a key component in a next generation server.
DPUs can be used as a stand-alone embedded processor, but they’re more often incorporated into a SmartNIC, a network interface controller used as a key component in a next-generation server.

![](https://blogs.nvidia.com/wp-content/uploads/2020/05/dpu-card.jpg.webp)

For example, some vendors use proprietary processors that don’t benefit from the broad Arm CPU ecosystem’s rich development and application infrastructure.

Others claim to have DPUs but make the mistake of focusing solely on the embedded CPU to perform data path processing.



# A Focus on Data Processing

That approach isn’t competitive and doesn’t scale, because trying to beat the traditional x86 CPU with a brute force performance attack is a losing battle. If 100 Gigabit/sec packet processing brings an x86 to its knees, why would an embedded CPU perform better?

Instead, the network interface needs to be powerful and flexible enough to handle all network data path processing. The embedded CPU should be used for control path initialization and exception processing, nothing more.

At a minimum, there 10 capabilities the network data path acceleration engines need to be able to deliver:

- Data packet parsing, matching and manipulation to implement an open virtual switch (OVS)
- RDMA data transport acceleration for Zero Touch RoCE
- GPUDirect accelerators to bypass the CPU and feed networked data directly to GPUs (both from storage and from other GPUs)
- TCP acceleration including RSS, LRO, checksum, etc.
- Network virtualization for VXLAN and Geneve overlays and VTEP offload
- Traffic shaping “packet pacing” accelerator to enable multimedia streaming, content distribution networks and the new 4K/8K Video over IP (RiverMax for ST 2110)
- Precision timing accelerators for telco cloud RAN such as 5T for 5G capabilities
- Crypto acceleration for IPSEC and TLS performed inline, so all other accelerations are still operational
- Virtualization support for SR-IOV, VirtIO and para-virtualization
- Secure Isolation: root of trust, secure boot, secure firmware upgrades, and authenticated containers and application lifecycle management
- These are just 10 of the acceleration and hardware capabilities that are critical to being able to answer yes to the question: “What is a DPU?”

# So what is a DPU? This is a DPU:

![](https://blogs.nvidia.com/wp-content/uploads/2020/05/bluefield-dpu.png.webp)

What's a DPU? This is a DPU, also known as a Data Processing Unit.

Many so-called DPUs focus on delivering just one or two of these functions.

The worst try to offload the datapath in proprietary processors.

While good for prototyping, this is a fool’s errand because of the scale, scope and breadth of data centers.

