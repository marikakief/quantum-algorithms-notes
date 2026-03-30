# Gate Teleportation Protocol

> **Source:** Gottesman & Chuang, arXiv:quant-ph/9908010
> **Tags:** #trick #teleportation #fault-tolerance #Clifford-hierarchy #gate-synthesis

## What it does
Implements a quantum gate $U$ by teleporting the input state through a pre-prepared entangled resource state, rather than applying $U$ directly.

## The trick
To apply an $n$-qubit gate $U$, prepare the resource state:

$$|\Psi^n_U\rangle = (I \otimes U)(|00\rangle + |11\rangle)^{\otimes n}$$

Teleport the input $|\psi\rangle$ through this state by performing Bell measurements on $|\psi\rangle$ and the upper halves of the EPR pairs. The output is:

$$|\psi_{\mathrm{out}}\rangle = R'_{xz}\, U\, |\psi\rangle$$

where $R'_{xz} = U R_{xz} U^\dagger$ is the teleportation byproduct conjugated by $U$. If $U \in \mathcal{C}_k$ (the $k$-th level of the [[Clifford Hierarchy for Fault-Tolerant Gate Classification|Clifford hierarchy]]), then $R'_{xz} \in \mathcal{C}_{k-1}$. The correction drops one level.

For $U \in \mathcal{C}_3$ (T gate, Toffoli): the correction is a Clifford gate, which is easy to perform fault-tolerantly on any stabiliser code. So the hard gate ($U$) is replaced by an easy correction plus an offline resource state.

## When to reach for it
- Fault-tolerant implementation of non-Clifford gates (T gate, Toffoli, controlled-$S$)
- Any situation where you can prepare a resource state offline but need the gate to act on live data
- Magic state injection in surface code architectures
- Conceptual foundation for understanding [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes|measurement-based quantum computation]]

## Complexity
One Bell measurement per qubit + one $\mathcal{C}_{k-1}$ correction. The hard part is preparing $|\Psi^n_U\rangle$ to sufficient fidelity. For the T gate, this is a single "magic state" $T|+\rangle$ that can be distilled from noisy copies.

## Caveat
The resource state $|\Psi^n_U\rangle$ for $\mathcal{C}_k$ gates with $k \geq 4$ is exponentially expensive in $k$ to prepare. In practice, only $k = 3$ (T gate) is used routinely. Also, the Bell measurement must be performed fault-tolerantly, which requires transversal operations across code blocks.

## Related notes
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]
- [[Clifford Hierarchy for Fault-Tolerant Gate Classification]]
- [[Offline Resource State Preparation for Fault Tolerance]]
- [[Byproduct Propagation in MBQC]]
