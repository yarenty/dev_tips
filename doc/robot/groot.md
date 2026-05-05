---
title: NVIDIA GR00T — humanoid-robot foundation model
main_link: https://developer.nvidia.com/isaac/gr00t
keywords: [groot, nvidia, humanoid-robot, foundation-model, isaac, ros, robotics]
status: reviewed
---

# NVIDIA GR00T — humanoid-robot foundation model

**Main link:** <https://developer.nvidia.com/isaac/gr00t>

## Summary

GR00T (Generalist Robot 00 Technology) is NVIDIA's general-purpose **foundation model for humanoid robots**, announced at GTC 2024. It's trained on multimodal demonstration data (videos, teleop, simulation) so a single network can drive walking, manipulation, and language-conditioned task execution across many humanoid bodies. NVIDIA ships GR00T together with the **Isaac** robotics stack (Isaac Sim, Isaac Lab, Project GROOT N1 open weights) and bridges to ROS 2 via NVIDIA's existing `isaac_ros_*` packages.

## Insight

This is the "GPT for humanoids" play — instead of bespoke controllers per platform / per skill, train one model and adapt it. Worth tracking if you build on humanoids (1X, Figure, Apptronik, Fourier, Boston Dynamics, Unitree); the open-weights **GR00T N1** release in 2025 made it real for hobbyists and labs (RTX-class GPUs are enough for fine-tune / inference). Less interesting if you're on industrial arms — for those Isaac Lab + classic RL is still the better workflow. The bridge story (GR00T as a high-level planner, ROS 2 / `isaac_ros_*` for low-level control + sensors) is what makes it actually deployable rather than a research demo.

Caveat: Isaac stack assumes NVIDIA hardware end-to-end (Jetson on the robot, RTX/dGPU for training, Omniverse for sim). Lock-in is real.

## Similar / related topics

- **Isaac Sim / Isaac Lab** — NVIDIA's GPU-accelerated robot simulator and RL training environment.
- **ROS 2** — the open-source middleware GR00T bridges into for sensor pipelines and low-level control.
- **OpenVLA, RT-2, Octo** — academic / open vision-language-action foundation models in the same space.
- **Boston Dynamics Spot SDK / Figure / 1X NEO** — humanoid platforms GR00T targets.
- [[nvidia_omniverse]] — companion 3D simulation/digital-twin platform from NVIDIA.

## Internal links

<!-- reviewed -->

- [[nvidia_omniverse]] — Companion NVIDIA simulation platform; GR00T training pipelines run on it.
- [[lerobot]] — Hugging Face's open robot-learning framework (related space, different vendor).

## Keywords

`#groot` `#nvidia` `#humanoid-robot` `#foundation-model` `#isaac` `#ros` `#robotics`

## References / raw notes

- NVIDIA Isaac GR00T product page: <https://developer.nvidia.com/isaac/gr00t>
- IEEE Spectrum: *NVIDIA Wants to Be the OS for Humanoid Robots*: <https://spectrum.ieee.org/nvidia-gr00t-ros>
- GR00T N1 open release blog: <https://blogs.nvidia.com/blog/gr00t-n1-open-humanoid-robot-foundation-model-simulation-frameworks/>
- Isaac Lab repo: <https://github.com/isaac-sim/IsaacLab>
- Isaac ROS packages: <https://github.com/NVIDIA-ISAAC-ROS>

### Why the ROS-2 bridge matters

GR00T is the *brain* (high-level perception + action policy). To run on a real humanoid you still need:

- a **real-time low-level controller** for joint actuation (typically EtherCAT / CAN);
- **sensor drivers** (cameras, IMU, depth, force-torque);
- a **safety / e-stop** layer;
- a way to **publish / subscribe** to all of those over a deterministic bus.

That's exactly what ROS 2 gives you, so NVIDIA's positioning is "GR00T policy on Jetson + ROS 2 underneath" rather than reinventing the middleware. The `isaac_ros_*` package family wraps the GPU-accelerated bits (depth, segmentation, AprilTag, VSLAM, …) as ROS 2 nodes so they slot into existing humanoid stacks.
