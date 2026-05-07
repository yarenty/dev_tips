---
title: LeRobot — HuggingFace's robot-learning library
main_link: https://github.com/huggingface/lerobot
keywords: [lerobot, huggingface, robotics, imitation-learning, reinforcement-learning, smolvla, pi0, act, foundation-models]
status: reviewed
review_date: 2026/05/03
---

# LeRobot — HuggingFace's robot-learning library

**Main link:** <https://github.com/huggingface/lerobot>

## Summary
LeRobot is HuggingFace's open library for **state-of-the-art ML on real-world robots**, in PyTorch. It bundles pretrained policies (ACT, Diffusion Policy, SmolVLA, π0/π0-FAST, VQ-BeT, …), human-collected datasets and simulation environments hosted on the HuggingFace Hub at <https://huggingface.co/lerobot>, and the training/eval glue to run them on affordable hardware (Koch arm, SO-100, Aloha, Stretch). The stated goal is to lower the barrier to entry to robotics so that everyone can contribute and benefit from shared datasets and pretrained models — the same playbook HF ran for NLP, applied to robot manipulation and locomotion.

## Insight
Reach for LeRobot when the brief is **"I want to do imitation learning or modern foundation-model robotics on a real arm without writing the data pipeline / policy code from scratch"**. The strongest sweet spots are: (1) record a few hundred teleoperated demonstrations of a manipulation task, train ACT or Diffusion Policy, deploy on the same arm; (2) fine-tune a vision-language-action foundation model (SmolVLA, π0) on your own task instead of training from scratch; (3) reuse community datasets directly off the HF Hub.

The 2024-2025 model releases are the centre of gravity:
- **ACT** (Action Chunking Transformer, from Aloha) — strong default for manipulation imitation learning.
- **Diffusion Policy** — diffusion-as-policy for visuomotor control; widely competitive baseline.
- **SmolVLA** — HF's small, open vision-language-action model designed to run on consumer hardware; the "Llama-7B moment" attempt for robotics.
- **π0 / π0-FAST** (from Physical Intelligence) — open-weights generalist VLA policy; LeRobot integrated quickly.

Vs the neighbours:
- **vs NVIDIA Isaac Lab / Isaac Sim** — Isaac is the heavyweight sim + RL stack with photorealistic rendering and GPU-parallelised env rollouts; LeRobot is lighter, more imitation-learning-shaped, more focused on real hardware.
- **vs [[../../robot/groot|groot]]** (NVIDIA GR00T) — GR00T is NVIDIA's parallel humanoid-robot foundation model + dev platform; same trend (foundation models for robots), different ecosystem and hardware tier.
- **vs Robomimic** — Robomimic is the academic predecessor for offline imitation-learning benchmarks; LeRobot is the better-maintained, hub-integrated, real-hardware-friendly successor.
- **vs Stable-Baselines3 + Gymnasium** — SB3 is the canonical RL library; LeRobot is imitation-learning-first and dataset-hub-integrated, not RL-first.
- **vs MuJoCo Menagerie** — Menagerie is a curated MJCF model zoo; complementary (you can use Menagerie scenes inside LeRobot pipelines).

Honest caveats: real-robot ML is brittle even with great tooling — calibration, kinematics, gripper variation, and sim-to-real gap will still bite; HF Hub bandwidth and storage are not free at scale; sim coverage is intentionally narrower than Isaac. The pretrained-policy story works best when your robot and task look reasonably like the demo data.

## Similar / related topics
- [[../../robot/README|robot]] — the broader robotics section in this vault.
- [[../../robot/groot|groot]] — NVIDIA's parallel humanoid-robot foundation-model platform.
- [NVIDIA Isaac Lab](https://developer.nvidia.com/isaac/sim) — heavyweight sim + RL stack.
- [Robomimic](https://robomimic.github.io/) — academic predecessor for offline imitation-learning benchmarks.
- [MuJoCo Menagerie](https://github.com/google-deepmind/mujoco_menagerie) — curated MJCF robot models for sim.

## Internal links
- [[../../robot/README|robot]]
- [[../../robot/groot|groot]]
- [[../llm/README|llm]]
- [[../agents/README|agents]]

## Keywords
`#lerobot` `#huggingface` `#robotics` `#imitation-learning` `#smolvla` `#pi0` `#act` `#foundation-models`

## References / raw notes

State-of-the-art Machine Learning for real-world robotics.

🤗 LeRobot aims to provide models, datasets, and tools for real-world robotics in PyTorch. The goal is to lower the barrier to entry to robotics so that everyone can contribute and benefit from sharing datasets and pretrained models.

Contains state-of-the-art approaches that have been shown to transfer to the real world, focused on imitation learning and reinforcement learning. Ships pretrained models, human-collected demonstration datasets, and simulation environments — so you can get started without assembling a robot.

Pretrained models and datasets live on HF: <https://huggingface.co/lerobot>.
