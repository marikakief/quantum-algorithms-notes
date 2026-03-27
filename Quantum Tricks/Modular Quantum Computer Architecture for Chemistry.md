
> **Source:** Reiher, Wiebe, Svore, Wecker, Troyer, arXiv:1605.03590 (Section III, Table II)
> **Tags:** #trick #architecture #fault-tolerant #surface-code #T-factory #resource-estimation

## What it does
Separates the quantum computer into functionally distinct modules — a small general-purpose processor and many special-purpose factories — to make fault-tolerant chemistry feasible despite enormous total qubit counts.

## The trick
Rather than one monolithic quantum processor, the architecture has three components:

1. **Main quantum processor**: holds the logical qubits of the simulation (~111 logical qubits for FeMoCo). General-purpose, fully error-corrected. This is the expensive, hard-to-build part.
2. **T factories**: dedicated devices that distill [[Magic State Distillation|magic states]] from noisy resource states using the 15-to-1 Bravyi-Kitaev protocol. Each factory is small (~$10^4$–$10^6$ physical qubits) and special-purpose. You need many of them (dozens to thousands), but they can be mass-produced.
3. **Rotation factories (QRot)**: build single-qubit rotations from T gates produced by the T factories. Only needed for Trotter-based approaches; [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] eliminates this layer.

Communication: T factories send magic states to rotation factories (quantum channels). Rotation factories send completed rotations to the main processor. A classical supercomputer orchestrates everything.

The key insight: most physical qubits ($>99\%$ in many configurations) are in the T factories, not the main processor. The processor is small. The factories are repetitive and can be manufactured as identical units.

## When to reach for it
- Any surface-code resource estimation where T-gate production is the bottleneck
- Useful framing for comparing architectures: "how many factories do I need?" is more actionable than "how many physical qubits total?"
- Applies to any fault-tolerant algorithm, not just chemistry

## Complexity
For FeMoCo (Reiher et al., Structure 1, 0.1 mHa, serial):

| Error rate | Processor qubits | T factories | Qubits per factory | Total physical qubits |
|---|---|---|---|---|
| $10^{-3}$ | $1.7 \times 10^6$ | 202 | $8.7 \times 10^5$ | $1.8 \times 10^8$ |
| $10^{-6}$ | $1.1 \times 10^5$ | 68 | $1.7 \times 10^4$ | $1.2 \times 10^6$ |
| $10^{-9}$ | $3.5 \times 10^4$ | 38 | $5.0 \times 10^3$ | $2.3 \times 10^5$ |

## Caveat
The modular model assumes quantum communication between factories and the main processor is possible with manageable overhead. In practice, inter-module communication has its own error rates and latency costs. Also, the factory counts scale linearly with the number of rotations done in parallel — PAR pushes factory counts to ~166,000, which strains the "mass production" framing.

Modern distillation protocols (e.g., Litinski's factories) are much more efficient than the 15-to-1 scheme used here, reducing factory sizes and counts by large factors.

## Related notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]
- [[Programmable Ancilla Rotations (PAR) for T-Depth Reduction]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
- [[Hybrid Classical-Quantum Chemistry Workflow]]
