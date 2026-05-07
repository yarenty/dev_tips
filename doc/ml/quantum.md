---
title: Quantum machine learning — landscape and honest framing
main_link: https://pennylane.ai/
keywords: [quantum, qml, pennylane, qiskit, vqc, quantum-kernel, hybrid, nisq]
status: reviewed
---

# Quantum machine learning — landscape and honest framing

**Main link:** <https://pennylane.ai/>

## Summary

Quantum machine learning (QML) is the intersection of quantum computing and ML — using parameterised quantum circuits as trainable models (variational quantum circuits / VQCs), quantum kernels for SVM-style classifiers, and hybrid classical-quantum architectures where a classical optimiser drives a quantum subroutine. **As of 2025 there is no demonstrated quantum advantage for any practical ML task**; the field is firmly in the NISQ (noisy intermediate-scale quantum) research era. What's worth tracking is (a) the steady hardware progress (IBM Heron / Condor, IonQ Forte / Tempo, Quantinuum H2, Atom Computing, PsiQuantum) that's pushing toward fault-tolerant systems in the next 5-10 years, and (b) the maturing software stacks (PennyLane, Qiskit Machine Learning, TensorFlow Quantum, NVIDIA cuQuantum) that will let mainstream ML practitioners actually use that hardware when it arrives.

## Insight

Be honest about the state of the field:

1. **No quantum advantage for ML yet.** Every claim of a real-world QML speedup as of 2025 has either been beaten by a classical algorithm (the "dequantisation" results of Tang and others), or has been demonstrated only on toy datasets where classical methods also work fine, or relies on hardware/data assumptions that don't hold in practice. The honest pitch is *not* "quantum makes ML faster" — it's "*if* fault-tolerant quantum computers arrive, certain models that are mathematically natural to express on them might be useful."
2. **NISQ noise dominates.** Today's quantum hardware has gate error rates of 10⁻²-10⁻⁴; a useful ML circuit needs millions to billions of gates. The error budget doesn't close. Variational methods (VQCs / QAOA) try to be noise-tolerant but in practice their advantage over classical neural-network analogues is unclear and trainability suffers from "barren plateaus" (gradient variance that vanishes exponentially with width).
3. **The reasons to learn it anyway:**
   - **Hardware *is* improving.** IBM's roadmap (Heron 156-qubit 2024, Flamingo 2025, Kookaburra 2026, Blue Jay 2033 fault-tolerant). IonQ Tempo (2025+, ~64 algorithmic qubits with high fidelity). Quantinuum's logical-qubit demos. Atom Computing's 1180-qubit system. PsiQuantum's 1M-qubit photonic ambition.
   - **Quantum-native problems** (quantum chemistry, materials, certain optimisation, cryptanalysis) are the actual likely first wins; QML may piggyback.
   - **Software stacks are stabilising.** PennyLane (Xanadu), Qiskit (IBM), Cirq + TensorFlow Quantum (Google), Q# (Microsoft), Braket SDK (AWS), cuQuantum (NVIDIA, classical simulation accelerated on GPUs). Easy to learn now, easy to deploy if/when hardware delivers.
   - **Hybrid classical-quantum** workflows (variational, kernel-based) are an active research area where some near-term *demonstrations* — even without speedup — are publishable and give intuition for the eventual real applications.

When (and only when) to invest serious time in QML in 2025:

- You're a researcher in quantum computing, quantum chemistry, or related; QML is a natural offshoot.
- You're at a quantum-hardware company or a major lab with a quantum collaboration.
- You're doing forward-looking R&D and want to be ready when the hardware does close the gap.
- You enjoy the math and want to think clearly about *why* most QML claims don't hold up.

When **not** to: any production ML problem, any "we should add quantum AI to our product" pitch, any short-horizon investment thesis. Classical ML eats this lunch by orders of magnitude.

## The four canonical software stacks

| Stack | Maintainer | Hardware backends | Best for |
|-------|-----------|-------------------|----------|
| **PennyLane** | Xanadu | All major (IBM, IonQ, Rigetti, AWS, Azure, photonic) + simulators | Cross-vendor research; auto-differentiation; PyTorch / JAX / TF integration |
| **Qiskit Machine Learning** | IBM | IBM Quantum (Heron etc.) | If you're on IBM hardware; most polished tutorials |
| **TensorFlow Quantum** | Google + collaborators | Cirq simulator, Google Quantum AI | TF-shop research; deeply tied to Cirq |
| **cuQuantum** | NVIDIA | GPU classical simulation | Simulating large quantum circuits on GPUs (the *only* way most current QML research works) |

Plus the cloud-vendor SDKs: **Amazon Braket** (multi-vendor backends), **Azure Quantum** (Microsoft Q# + multi-vendor), **AWS Braket Hybrid Jobs**.

## Three QML patterns worth knowing

1. **Variational Quantum Circuit (VQC)** — a parameterised quantum circuit `U(θ)` whose output expectation value is the model prediction; gradients computed via the parameter-shift rule; θ trained with a classical optimiser. The hello-world of QML. See PennyLane's "QNode" abstraction.
2. **Quantum Kernel** — use the inner product of quantum-state-encoded inputs as the kernel for an SVM. Theoretically motivated (Havlíček et al. 2019); in practice not yet shown to outperform classical kernels on practical tasks.
3. **Hybrid classical-quantum** — a classical neural network feeds inputs to a quantum subroutine (or vice versa), trained end-to-end. The most flexible and the most likely shape of any eventual practical QML.

## Similar / related topics

- **PennyLane** — Xanadu's cross-vendor differentiable QML framework; the most popular default.
- **Qiskit Machine Learning** — IBM's QML extension to Qiskit.
- **TensorFlow Quantum** — Google's TF-integrated framework on top of Cirq.
- **NVIDIA cuQuantum** — GPU-accelerated classical simulation; how most QML actually runs in 2025.
- **Quantum chemistry / VQE** — adjacent and more likely to deliver near-term wins than ML.
- **Tensor networks** (MPS, PEPS, MERA) — classical simulation methods that share mathematical structure with QML; often beat shallow QML on the same problems.

## Internal links

- [[README|ml]] — ML section landing.
- [[fundamentals/no_deep_learning_needed|no_deep_learning_needed]] — adjacent in spirit: "what's the simplest method that works?". For QML in 2025, that's almost always a classical baseline.
- [[fundamentals/pattern_learning|pattern_learning]] — meta-topic landing.
- [[compilers/README|compilers]] — adjacent: ML compiler section (cuQuantum is GPU-side; the classical-simulator path lives next door).

## Keywords

`#quantum` `#qml` `#pennylane` `#qiskit` `#vqc` `#quantum-kernel` `#hybrid` `#nisq`

## References / raw notes

### Software stacks

- **PennyLane** (Xanadu) — <https://pennylane.ai/> — *the* cross-platform pick. Auto-differentiation across PyTorch / JAX / TF / NumPy backends; ~30 hardware/simulator plugins.
- **Qiskit Machine Learning** (IBM) — <https://qiskit-community.github.io/qiskit-machine-learning/>.
- **Cirq** + **TensorFlow Quantum** (Google) — <https://www.tensorflow.org/quantum>.
- **NVIDIA cuQuantum** — <https://developer.nvidia.com/cuquantum-sdk>. The *practical* way to run QML in 2025: simulate on GPUs.
- **Q#** (Microsoft) — <https://learn.microsoft.com/en-us/azure/quantum/>.
- **Amazon Braket SDK** — <https://aws.amazon.com/braket/>.

### Hardware (2024-2025 state of the art)

- **IBM Quantum** — Heron (156q, 2024); Flamingo (2025); Condor 1121q research chip. Roadmap to fault-tolerant *Blue Jay* by 2033.
- **IonQ** — Forte (~36 algorithmic qubits, 2024); Tempo and Tempo 2 (2025+).
- **Quantinuum** — H2 trapped-ion; recent logical-qubit demos with Microsoft.
- **Atom Computing** — 1180-qubit neutral-atom system (2024).
- **Rigetti** — Ankaa-3 (84q superconducting).
- **PsiQuantum** — photonic; chasing 1M physical qubits for fault tolerance.
- **Pasqal / QuEra** — neutral-atom analog QPUs; well-suited for combinatorial optimisation.

### Foundational QML papers / surveys

- Biamonte et al. 2017 — *Quantum Machine Learning.* Nature 549, 195. The canonical survey.
- Havlíček et al. 2019 — *Supervised learning with quantum-enhanced feature spaces.* Nature 567, 209. The quantum-kernel paper.
- Schuld & Killoran 2019 — *Quantum machine learning in feature Hilbert spaces.* PRL 122. Kernel-method framing.
- McClean et al. 2018 — *Barren plateaus in quantum neural network training landscapes.* The trainability problem.
- Cerezo et al. 2022 — *Variational quantum algorithms.* Nat Rev Phys.

### Honest critique

- Tang 2019 — *A quantum-inspired classical algorithm for recommendation systems.* The original "dequantisation" result that ate one of the headline QML speedup claims; followed by a series of similar dequantisations of HHL-shaped algorithms.
- Schuld 2022 — *Is quantum advantage the right goal for quantum machine learning?* (Perspective). Argues for reframing the field away from speedup claims.
- Various 2023-2024 critique papers under "barren plateaus", "no advantage on practical data", "dequantisation".

### Reading-list shortcut

- **PennyLane Codebook** — <https://codebook.xanadu.ai/> — interactive tutorial, the best entry point.
- **Qiskit Textbook** — <https://github.com/Qiskit/textbook> — broader quantum-computing intro that includes QML.
- **Maria Schuld's tutorials and talks** — Schuld is the most articulate QML researcher on what the field can and can't deliver; her talks on YouTube are recommended.
- **Quantum.country** — <https://quantum.country/> — Andy Matuschak / Michael Nielsen's spaced-repetition essay on quantum computing fundamentals (not ML-specific but the best foundational explainer).
