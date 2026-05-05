# Ancilla-Sewing of Local Inversions

> **Source:** Huang, Liu, Broughton, Kim, Anshu, Landau, McClean, arXiv:2401.10095
> **Tags:** #trick #circuit-learning #shallow-circuits #heisenberg-picture #swap

## What it does

Builds a global representation of an unknown shallow circuit from locally learned pieces, avoiding direct optimization over the full circuit.

## The trick

For each qubit $i$, learn the local Heisenberg observables $U^\dagger P_i U$ for $P\in\{X,Y,Z\}$. Package them into a system-ancilla operator

$$
W_i = \sum_{P\in\{I,X,Y,Z\}} U^\dagger P_i U \otimes P_i,
$$

which equals the conjugated local swap $U^\dagger S_i U$ in the exact case. Then use the identity

$$
U\otimes U^\dagger = S\prod_i (U^\dagger S_i U)
$$

with $S$ the global swap between system and ancilla. Replacing each exact factor by its learned approximation lets you assemble a learned dilation of $U$ from pieces that are each locally accessible.

The win is that local consistency is easy while global consistency is hard. Sewing sidesteps the hard part.

## When to reach for it

- You need to learn or certify a shallow circuit whose Heisenberg lightcones stay small.
- Direct variational fitting of the whole circuit is unstable or non-convex.
- You can estimate local Heisenberg observables but not the whole unitary.

## Complexity

For constant-depth circuits, each learned local observable has constant support, so the local learning remains polynomial. The final dilation uses an ancilla copy of the system.

## Caveat

The method breaks once lightcones grow. For logarithmic-depth circuits the local operators are no longer small, and the whole construction loses efficiency.

## Related notes

- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]
- [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]]
- [[Operator-to-State Mapping for Heisenberg Evolution]]
