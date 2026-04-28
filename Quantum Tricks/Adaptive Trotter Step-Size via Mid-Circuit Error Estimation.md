# Adaptive Trotter Step-Size via Mid-Circuit Error Estimation

> **Source:** Zhao, Zhou, Childs, arXiv:2406.02379
> **Tags:** #trick #hamiltonian-simulation #trotter #adaptive-algorithms #product-formulas #shadow-tomography

## What it does

Uses checkpoint measurements during a [[Product Formulas|product-formula]] simulation to estimate the next segment's Trotter error, then chooses the next segment's step count from that estimate instead of from a global worst-case bound.

## The trick

Suppose the total evolution time is split by checkpoints

$$
0=t_0 < t_1 < \cdots < t_T < t_{T+1}=t.
$$

At checkpoint $t_i$, prepare copies of the current approximate state $|\phi(t_i)\rangle$ and estimate the upcoming one-segment error.

There are two estimators in the paper.

1. **Purity-based estimator.** Use classical shadows to estimate purities of the local RDMs entering the entropy/distance-based bounds. For lattice Hamiltonians, this needs about $M=O(N^4\log N)$ samples at the precision relevant to the bound.
2. **Direct observable estimator.** Estimate
   $$
   \sum_{j,j'} \langle \phi(t_i)|E_j^\dagger E_{j'}|\phi(t_i)\rangle
   $$
   directly, where each term is a linear combination of $O(1)$ Pauli strings. For lattice Hamiltonians this brings the sample count down to $M=O(N^2)$.

Given the estimated error for the next interval $\Delta_i=t_{i+1}-t_i$, choose the Trotter number $r(\Delta_i)$ so that the allocated error budget for that interval is met, then continue.

The paper's Algorithm 1 is exactly this loop.

## When to reach for it

- When entanglement grows during the evolution, so a fixed worst-case step count is too conservative late in the run
- When you can afford checkpoint measurements but not full-blown state tomography
- When simulating thermalizing local systems where local entropy is expected to increase over time

## Complexity

The useful implementation is the direct-observable estimator, with about $M=O(N^2)$ shadow samples per checkpoint for lattice Hamiltonians. The gate cost then interpolates between worst-case early-time behavior and average-case late-time behavior.

## Caveat

- The protocol is only attractive with the direct-observable estimator. The naive purity-based route is much heavier.
- It assumes the checkpoint estimate remains informative over the next interval; this is plausible in monotone-entangling regimes but is not a worst-case guarantee.
- Mid-circuit measurements and repeated state preparation still carry real overhead.

## Related notes

- [[Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) — Paper Notes]]
- [[Entanglement-Entropy-Dependent Trotter Error Bound]]
- [[k-Uniformity Convergence to Average-Case Error]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
