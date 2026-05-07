---
title: Top ML / DL learning resources
main_link: https://karpathy.ai/zero-to-hero.html
keywords: [courses, learning, karpathy, fast-ai, andrew-ng, reading-list]
status: reviewed
---

# Top ML / DL learning resources

**Main link:** <https://karpathy.ai/zero-to-hero.html>

## Summary

A curated reading list of the most-recommended courses and self-study resources for getting from
zero to working ML/DL competence. The headline pick for 2024-2025 is Andrej Karpathy's
**Neural Networks: Zero to Hero** — a YouTube lecture series that builds backprop, MLPs, makemore,
WaveNet and a working GPT clone in PyTorch from scratch, with all code on GitHub. Around it sit
the classics (fast.ai practical course, Andrew Ng's Coursera Specialisation, MIT 6.S191) plus the
self-paced curated repos surveyed below (Virgilio, MLCourse, ProjectLearn, Best-of ML Python).

## Insight

Three honest opinions worth keeping in mind when picking a starting point:

1. **Karpathy's "Zero to Hero" is the best modern entry point** if you can write Python and remember
   high-school calculus. You'll come out understanding what backprop, attention, and a Transformer
   actually *do* at the tensor-shape level — knowledge that LLM-API-only learners systematically lack.
2. **fast.ai is the right complement** — top-down "train a working model in lesson 1" pedagogy that
   makes you productive with vision/NLP/tabular workflows in days, not months. Pair it with Karpathy
   for "from-scratch" depth.
3. **Andrew Ng's Coursera Specialisation** (Deep Learning, Machine Learning) is the best option for
   beginners who want a structured course with quizzes and certificates, and don't yet have the
   Python chops to keep up with Karpathy. Don't dismiss it for being slow-paced.

The Virgilio / MLCourse / Best-of-ML-Python-style curated repos are useful as **indexes**, but
beware: several of them stopped being maintained in 2019-2022 and now point at libraries with
half-broken APIs or post-fork upstreams. Cross-check anything you actually try to install.

## Similar / related topics

- **fast.ai** — Jeremy Howard's practical-deep-learning course; top-down opposite of Karpathy.
- **Andrew Ng's ML / DL Specialisations** (Coursera) — the polished beginner option with quizzes and certificates.
- **MIT 6.S191** — annual *Introduction to Deep Learning* lecture series; freely on YouTube.
- **CS231n / CS224N** (Stanford) — the canonical computer-vision and NLP graduate courses; lecture videos free, problem sets gold.
- **Hugging Face course** — the right next step after Karpathy if you'll be working with pretrained Transformers.

## Internal links
- [[../README|ml]] — section landing.
- [[../frameworks/README|frameworks]] — what these courses teach you to use (PyTorch, JAX, TF).
- [[../llm/README|llm]] — sibling section; Karpathy's later lectures lead directly here.
- [[no_deep_learning_needed]] — counterpoint reading: when *not* to use the things these courses teach.

## Keywords

`#courses` `#learning` `#karpathy` `#fast-ai` `#andrew-ng` `#reading-list`

## References / raw notes

### Headline pick — Karpathy's *Neural Networks: Zero to Hero*

[karpathy.ai/zero-to-hero.html](https://karpathy.ai/zero-to-hero.html) — a course by Andrej Karpathy
on building neural networks, from scratch, in code. Starts with backprop fundamentals
(*micrograd*) and builds up to a modern Transformer (*nanoGPT*).

Syllabus highlights:

- **2h25m** *The spelled-out intro to neural networks and backpropagation: building micrograd* — most step-by-step explanation of backprop in existence.
- **1h57m** *The spelled-out intro to language modeling: building makemore* — bigram character LM, intro to `torch.Tensor`.
- **1h15m** *Building makemore Part 2: MLP* — multi-layer perceptron LM + ML basics (LR tuning, train/dev/test, over/underfitting).
- **1h55m** *Part 3: Activations & Gradients, BatchNorm* — internals of deep MLPs, BatchNorm motivation.
- **1h55m** *Part 4: Becoming a Backprop Ninja* — manual backprop through the full network.
- **56m** *Part 5: Building a WaveNet* — tree-like deeper network.
- **1h56m** *Let's build GPT: from scratch, in code, spelled out* — implements GPT following *Attention is All You Need*.

Prerequisites: solid Python; intro-level math (derivative, gaussian).

### Curated reading-list repos

1. **[Virgilio](https://github.com/virgili0/Virgilio)** — three-tier (Paradiso/Purgatorio/Inferno)
   data-science learning path, theory → fundamentals → applications.
2. **[MLCourse](https://mlcourse.ai/)** — Yury Kashnitsky / OpenDataScience; ten-topic ML curriculum
   with notebooks, assignments, and video lectures. Russian track maintained; English track stopped
   ~2019 but still relevant for fundamentals.
3. **[ProjectLearn](https://projectlearn.io/)** — curated tutorial-project list across web/mobile/ML;
   the ML/AI section is the relevant one here.
4. **[Deepkapha tutorials](https://deepkapha.ai/)** — curated DL tutorials and DL-blog index.
5. **[Best-of ML Python](https://github.com/ml-tooling/best-of-ml-python)** — auto-updated ranked
   list of ML Python packages by category. The most useful of the five for "what library should I
   try?" questions.

### Other canonical resources worth knowing

- [fast.ai practical-deep-learning course](https://course.fast.ai/) — top-down pedagogy.
- [Andrew Ng — Deep Learning Specialization](https://www.coursera.org/specializations/deep-learning) (Coursera).
- [MIT 6.S191 — Introduction to Deep Learning](http://introtodeeplearning.com/).
- [Stanford CS231n](http://cs231n.stanford.edu/) — Computer vision.
- [Stanford CS224N](http://web.stanford.edu/class/cs224n/) — NLP with deep learning.
- [Hugging Face course](https://huggingface.co/learn) — Transformers / LLMs / Diffusion / RL.
