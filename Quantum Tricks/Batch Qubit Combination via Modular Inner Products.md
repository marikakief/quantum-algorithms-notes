# Batch Qubit Combination via Modular Inner Products

> **Source:** Oded Regev, arXiv:quant-ph/0406151 (2004)
> **Tags:** #trick #quantum-sieve #dihedral-HSP #space-efficient

## What it does
Combines $l + 4$ randomly-labelled qubit states to produce one qubit state with $l$ fewer random bits in its label, using only polynomial space. Replaces the pile-based collision search in [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg's quantum sieve]].

## The trick
Given $l + 4$ qubits $|\psi_{y_j}\rangle = |0\rangle + e^{2\pi i d y_j / N}|1\rangle$ with known random labels $y_1, \ldots, y_{l+4}$:

1. **Tensor and compute:** Form the $(l+4)$-qubit state $\sum_{\vec{b}} e^{2\pi i d \langle \vec{b}, \vec{y}\rangle/N} |\vec{b}\rangle$. Compute $\langle \vec{b}, \vec{y}\rangle \bmod 2^l$ into an ancilla (using the known $\vec{y}$).

2. **Measure modular sum:** Measuring the ancilla gives $z$, collapsing to a superposition over $\vec{b}$-strings satisfying $\langle \vec{b}, \vec{y}\rangle \equiv z \pmod{2^l}$.

3. **Classical enumeration:** Brute-force find all $\vec{b} \in \{0,1\}^{l+4}$ with $\langle \vec{b}, \vec{y}\rangle \equiv z$. With constant probability (Chebyshev), the count $m$ is between 2 and 32.

4. **Project and relabel:** Project onto $\text{span}(|\vec{b}_1\rangle, |\vec{b}_2\rangle)$ for two solutions, then relabel to get $|\psi_w\rangle$ with $w = \langle \vec{b}_2 - \vec{b}_1, \vec{y}\rangle$, which has $l$ trailing zeros.

The point: we've zeroed out $l$ bits of the label without needing to store $2^l$ qubits waiting for collisions. The classical computation costs $2^{O(l)}$ but uses no quantum memory beyond the $l + 4$ qubits being processed.

## When to reach for it
- When applying a [[Quantum Sieve for Labelled Qubits|quantum sieve]] but space is the binding constraint (polynomial quantum memory required)
- Hidden subgroup / hidden shift problems where the sieve approach works but the $2^{O(\sqrt{n})}$ space of Kuperberg's method is prohibitive
- Trading classical computation time for quantum space — the batch combination shifts work from quantum storage to classical enumeration

## Complexity
- Classical time per combination: $O(2^l)$ (brute-force enumeration of $\{0,1\}^{l+4}$)
- Quantum space per combination: $O(l)$ qubits
- Success probability: constant per attempt (via Chebyshev on pairwise-independent indicators)

## Caveat
- The $2^{O(l)}$ classical brute-force step is the performance bottleneck. The total algorithm time is $2^{O(\sqrt{n \log n})}$ vs Kuperberg's $2^{O(\sqrt{n})}$ — the $\log n$ penalty comes from $l = O(\sqrt{n \log n})$.
- The projection onto two solutions (step 4) succeeds only if the measurement in step 2 gives $m \geq 2$ solutions — guaranteed with constant probability but not certainty.
- Wastes $l + 3$ qubits per successful combination. The pipeline must produce $l^{O(k)}$ total oracle calls.
- Could potentially be improved by smarter enumeration of $\vec{b}$ (e.g., subset-sum algorithms rather than brute force).

## Related notes
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Quantum Sieve for Labelled Qubits]]
- [[Coset State to Labelled Qubit Reduction]]
