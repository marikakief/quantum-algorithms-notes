# Clifford Hierarchy for Fault-Tolerant Gate Classification

> **Source:** Gottesman & Chuang, arXiv:quant-ph/9908010
> **Tags:** #trick #Clifford-hierarchy #fault-tolerance #gate-classification #complexity

## What it does
Classifies quantum gates by how hard they are to implement fault-tolerantly, using a recursive hierarchy that directly determines the resource overhead for [[Gate Teleportation Protocol|gate teleportation]].

## The trick
Define the Clifford hierarchy recursively:

$$\mathcal{C}_1 = \text{Pauli group}, \quad \mathcal{C}_k = \{U \mid U \mathcal{C}_1 U^\dagger \subseteq \mathcal{C}_{k-1}\}$$

- $\mathcal{C}_1$ (Paulis): trivially fault-tolerant on any code
- $\mathcal{C}_2$ (Cliffords: CNOT, $H$, $S$): transversal on stabiliser codes; classically simulable by [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Gottesman-Knill]]
- $\mathcal{C}_3$: T gate ($\pi/8$), Toffoli, controlled-$S$ — the first level requiring magic states
- $\mathcal{C}_k$: contains $\pi/2^k$ rotations; exponentially expensive in $k$ to implement

The hierarchy matters because gate teleportation through a $\mathcal{C}_k$ resource state produces a $\mathcal{C}_{k-1}$ byproduct correction. So the "cost" of a gate is determined by its level: each level adds one round of resource state preparation and correction.

## When to reach for it
- Determining the fault-tolerance overhead for a specific gate
- Understanding why T gates dominate resource estimates (they're the lowest non-Clifford level)
- Analysing which gates are "free" (Cliffords) vs. "expensive" (non-Cliffords) in a given error-correcting code
- Designing gate sets: $\mathcal{C}_2$ + any $\mathcal{C}_3$ gate = universal

## Complexity
$\mathcal{C}_2$ gates: constant overhead (transversal). $\mathcal{C}_3$ gates: one round of magic state preparation + distillation. $\mathcal{C}_k$ gates for $k > 3$: $k-2$ nested levels of resource preparation — exponential in $k$.

## Caveat
The hierarchy doesn't capture all useful gates — some important gates (like arbitrary rotations) aren't in any finite level of the hierarchy and must be approximated by sequences of $\mathcal{C}_3$ gates (via [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Solovay-Kitaev]] or more efficient methods). The T-count of a circuit — the number of T gates after Clifford simplification — is the standard measure of fault-tolerant cost.

## Related notes
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]
- [[Gate Teleportation Protocol]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
