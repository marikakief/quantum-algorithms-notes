# Cluster State as Universal Entanglement Resource

> **Source:** Raussendorf & Briegel, arXiv:quant-ph/0010033
> **Tags:** #trick #MBQC #cluster-state #entanglement #universality

## What it does
Provides a single, uniformly prepared entangled state — a universal substrate for any quantum computation.

## The trick
Prepare all qubits in $|+\rangle$ on a 2D lattice, then apply controlled-$Z$ gates between all nearest-neighbour pairs:

$$|\Phi\rangle_C = \prod_{\langle a,a'\rangle} CZ_{a,a'} |+\rangle^{\otimes n}$$

The resulting **cluster state** satisfies stabiliser equations $\sigma_x^{(a)} \prod_{a' \in \mathrm{ngbh}(a)} \sigma_z^{(a')} |\Phi\rangle_C = \pm |\Phi\rangle_C$ for every site $a$. These equations mean that single-qubit measurements on some qubits deterministically implement logical operations on the remaining qubits. The measurement pattern encodes the computation — the cluster state itself is computation-agnostic.

Any quantum circuit can be "imprinted" on the cluster by choosing appropriate measurement bases. The cluster is consumed (destroyed) in the process — hence "one-way" quantum computer.

## When to reach for it
- Designing a quantum computer architecture where entanglement generation is easy but deterministic multi-qubit gates are hard (e.g., photonic systems, some atomic platforms)
- Situations where you want to decouple resource preparation from computation logic
- When you need a universal resource state for delegation or blind quantum computation protocols

## Complexity
Preparing the cluster state requires $O(|E|)$ controlled-$Z$ gates where $|E|$ is the number of lattice edges. For a 2D lattice with $n$ qubits, this is $O(n)$ since the degree is constant. The gates are all commuting, so they can be applied in constant depth.

## Caveat
The cluster state must be maintained coherently until all measurements are complete. Decoherence during the measurement phase degrades the computation. The required cluster size scales polynomially with the circuit being simulated, so large computations need large clusters.

## Related notes
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]]
- [[Measurement-Driven Computation via Stabiliser Relations]]
- [[Byproduct Propagation in MBQC]]
- [[Adaptive Measurement Bases for Feed-Forward]]
