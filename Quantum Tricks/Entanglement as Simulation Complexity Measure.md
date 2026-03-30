# Entanglement as Simulation Complexity Measure

> **Source:** Vidal, arXiv:quant-ph/0301063
> **Tags:** #trick #entanglement #classical-simulation #quantum-advantage #complexity

## What it does
Uses $E_\chi = \log_2 \chi$ (the log of the maximum Schmidt rank across all bipartitions) as a direct proxy for classical simulation cost, giving a necessary condition for quantum computational advantage.

## The trick
For an $n$-qubit pure state, the classical description complexity in [[MPS Canonical Form for Efficient State Tracking|MPS form]] is:

$$\text{parameters} \approx n \cdot \exp(E_\chi)$$

This gives an immediate classification:
- $E_\chi = O(\log n)$ → poly$(n)$ classical simulation (no exponential quantum advantage possible)
- $E_\chi = \omega(\log n)$ but $o(n)$ → superpolynomial but subexponential simulation cost
- $E_\chi = \Theta(n)$ → exponential simulation cost (quantum advantage is *possible* but not guaranteed)

The key insight: entanglement growth is *necessary* for exponential quantum speedup. If $\chi$ stays poly$(n)$ throughout a computation, a classical computer can track the full state efficiently.

## When to reach for it
- Proving that a class of quantum computations cannot provide exponential speedups (show entanglement stays bounded)
- Designing classical simulation algorithms: if you can bound $\chi$ for a physical system, you know MPS/tensor-network methods will work
- Understanding the boundary of quantum advantage: which problems produce computations with polynomially bounded entanglement?

## Complexity
The simulation cost is poly$(n) \cdot$ poly$(\chi)$ for a poly$(n)$-gate circuit, where $\chi = 2^{E_\chi}$.

## Caveat
This is a *necessary* condition for quantum advantage, not sufficient. High entanglement doesn't guarantee a speedup — it just means the MPS simulation fails. Other classical methods (e.g., [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|stabiliser simulation]], Monte Carlo) might still work for highly entangled states with special structure.

Also, $E_\chi$ is discontinuous: states arbitrarily close to $|\Psi\rangle$ can have very different Schmidt ranks. This means the measure is fragile under perturbation, though truncated versions $E_{\chi_\delta}$ (keeping only Schmidt values above a threshold) are better behaved.

For mixed states, $\chi$ measures total correlations (classical + quantum), which is a weaker statement about quantum advantage.

## Related notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — TEBD exploits bounded $\chi$ for efficient 1D dynamics
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]
