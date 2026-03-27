
> **Source:** Gidney (2018), Kitaev, Shen & Vyalyi (2002); compiled in Sanders, Berry et al., arXiv:2007.07391
> **Tags:** #trick #phase-gradient #rotation #fault-tolerant #arithmetic

## What it does
Converts an arbitrary $Z$-rotation by a quantum-register angle into a sequence of Toffoli gates (additions), avoiding per-rotation synthesis overhead. A single reusable ancilla state enables all rotations in a circuit.

## The trick
Prepare (once, before computation) the phase gradient state:

$$|\phi\rangle = \frac{1}{\sqrt{2^{b}}} \sum_{k=0}^{2^b - 1} e^{-2\pi i k / 2^b} |k\rangle = \bigotimes_{j=1}^{b} \frac{|0\rangle + e^{-2\pi i / 2^j} |1\rangle}{\sqrt{2}}$$

Adding an integer $\ell$ into this register produces the phase kickback $e^{2\pi i \ell / 2^b} |\phi\rangle$. To rotate a qubit by angle $\theta = 2\pi \ell / 2^b$:

1. Use the target qubit to control whether addition or subtraction of $\ell$ is performed (toggle via CNOTs before/after, following the two's complement trick).
2. This applies $e^{-i\theta Z}$ to the target qubit.

The addition into the phase gradient state costs $b - 2$ Toffolis ($b - 3$ if $\ell$ is classical). When the rotation angle is a product $\gamma \cdot k$ where $k$ is in a quantum register and $\gamma$ is classical, represent $\gamma$ as a sum of $\leq \lceil(n+1)/2\rceil$ signed powers of 2 and perform that many bit-shifted additions. Total cost: $\sim b(n + b_{\text{pha}})/2$ Toffolis.

## When to reach for it
- Fault-tolerant circuits where many rotations by quantum-register-controlled angles are needed (QAOA cost function phasing, QSA transition probability rotations, adiabatic driving fields).
- Any time you'd otherwise need controlled rotation synthesis with $O(\log(1/\varepsilon))$ T gates per rotation.
- Especially useful when the same $|\phi\rangle$ state can be reused across the entire computation.

## Complexity
- Preparation: $O(b \log(b/\varepsilon))$ T gates (one-time cost, negligible in context).
- Per rotation: $b - 2$ Toffolis (quantum angle) or $b - 3$ Toffolis (classical angle).
- Per multiplication-into-phase: $b \cdot \lceil(n+1)/2\rceil$ Toffolis where $n$ is the number of bits of the classical multiplier.
- Ancilla: $b$ qubits for $|\phi\rangle$ (persistent, reusable).

## Caveat
Angles are discretized to multiples of $2\pi / 2^b$. To achieve rotation error $\leq \varepsilon$, need $b = O(\log(1/\varepsilon))$ plus overhead for multiplication precision (extra $O(\log n)$ bits). The state $|\phi\rangle$ must be maintained coherently throughout the computation.

## Related notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Adaptive QROM for Function Evaluation]]
