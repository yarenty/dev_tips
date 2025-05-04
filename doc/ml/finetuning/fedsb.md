# Fed-SB

paper: https://arxiv.org/pdf/2502.15436v1

https://github.com/CERT-Lab/fed-sb

Fed-SB: A Silver Bullet for Extreme Communication Efficiency and Performance in (Private) Federated LoRA Fine-Tuning

## Introduction
Low-Rank Adaptation (LoRA) has become ubiquitous for efficiently fine-tuning foundation models. However, federated fine-tuning of LoRA remains challenging due to non-optimal updates arising from traditional federated averaging of individual adapters. Existing solutions either incur prohibitively high communication cost that scales linearly with the number of clients or suffer from performance degradation due to limited expressivity. We introduce Federated Silver Bullet (Fed-SB), a novel approach for federated fine-tuning of LLMs using LoRA-SB, a recently proposed low-rank adaptation method. LoRA-SB learns a rxr matrix between B and A, while keeping other components fixed. Direct averaging of R guarantees exact updates, substantially reducing communication cost, which remains independent of the number of clients, and enables scalability. Fed-SB achieves state-of-the-art performance across commonsense reasoning, arithmetic reasoning, and language inference tasks while reducing communication costs by up to 230x. In private settings, Fed-SB further improves performance by (1) reducing trainable parameters, thereby lowering the noise required for differential privacy, and (2) avoiding noise amplification introduced by other methods. Overall, Fed-SB establishes a new Pareto frontier in performance vs. communication cost, offering an efficient and scalable solution for both private and non-private federated fine-tuning.


![](https://github.com/CERT-Lab/fed-sb/raw/main/assets/results.png)

