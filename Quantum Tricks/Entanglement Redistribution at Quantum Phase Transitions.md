# Entanglement Redistribution at Quantum Phase Transitions

> **Source:** Osborne and Nielsen, arXiv:quant-ph/0202162
> **Tags:** #trick #entanglement #quantum-phase-transition #entanglement-monogamy

## What it does
Explains why pairwise entanglement between nearby sites can *decrease* at a quantum critical point even though the total entanglement in the system is maximised.

## The trick
At a quantum phase transition, correlations develop on all length scales. Each site must share entanglement with all other sites in the lattice, not just its nearest neighbours. Because entanglement obeys monogamy constraints (the CKW inequality: $C_{A,B}^2 + C_{A,C}^2 \leq C_{A,BC}^2$ for qubits), distributing entanglement to more partners necessarily reduces the pairwise entanglement with any single partner.

The diagnostic is: the **single-site von Neumann entropy** (entanglement of one site with the rest of the chain) peaks at criticality, while the **nearest-neighbour concurrence** peaks *below* the critical point. The next-nearest-neighbour concurrence, being much smaller, can still peak at or near the critical point because it was negligible to begin with.

This gives a physical picture of the entanglement structure at criticality: entanglement is delocalised, spread thinly across all length scales, constrained by monogamy.

## When to reach for it
- Interpreting entanglement measures near quantum phase transitions — if you see pairwise entanglement *decrease* at a critical point, this explains why
- Designing entanglement witnesses for QPTs: use single-site entropy or entanglement entropy of a *block* rather than pairwise concurrence, since the former captures the total entanglement budget
- Understanding why tensor network methods (MPS/DMRG) struggle at criticality: the entanglement is distributed across all scales, requiring large bond dimension — this is the same delocalisation phenomenon

## Complexity
No computational cost — this is a conceptual/analytical tool. Computing the von Neumann entropy requires diagonalising the reduced density matrix (trivial for a single qubit; for a block of $\ell$ sites, $O(2^\ell)$ in general, but efficient via transfer matrix or Jordan-Wigner in free-fermion models).

## Caveat
The single-site entropy is *not* universal — it has different scaling on the two sides of the transition in the transverse Ising model. The *block* entanglement entropy (Calabrese-Cardy, 2004) is the universal quantity: $S_\ell \sim \frac{c}{3}\log\ell$ with central charge $c$. The redistribution picture is correct, but the right entanglement measure for universality is the block entropy, not the single-site entropy.

Also, entanglement monogamy as stated applies to qubits (CKW inequality). For higher-dimensional local Hilbert spaces, monogamy constraints are weaker or different.

## Related notes
- [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]]
