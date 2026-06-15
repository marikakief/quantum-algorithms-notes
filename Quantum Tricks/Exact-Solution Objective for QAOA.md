# Exact-Solution Objective for QAOA

> **Source:** Boulebnane--Montanaro, arXiv:2208.06909
> **Tags:** #trick #QAOA #SAT #constraint-satisfaction #sampling

## What it does
Uses QAOA as a sampler for exact satisfying assignments, rather than as an approximate optimiser of expected energy.

## The trick
Given a SAT instance $\sigma$, define the cost Hamiltonian

$$
H[\sigma]=\sum_y |\{j:y\not\models \sigma_j\}|\,|y\rangle\langle y|.
$$

The target is the zero-energy projector

$$
\{H[\sigma]=0\},
$$

and the objective is

$$
p_{\mathrm{succ}}(\sigma)=
\langle\Psi(\sigma,\beta,\gamma)|\{H[\sigma]=0\}|\Psi(\sigma,\beta,\gamma)\rangle.
$$

Repeatedly sample from QAOA until a satisfying assignment appears. The expected sample count is

$$
T(\sigma)=1/p_{\mathrm{succ}}(\sigma).
$$

## Why it matters
Expected energy can look good while the probability of an exact solution remains tiny. For SAT near the threshold, the exponent of $p_{\mathrm{succ}}$ is the meaningful quantity.

## When to reach for it
Use for CSPs where the desired output must satisfy all constraints, not merely many of them: SAT, exact cover, planted CSPs, and threshold random ensembles.

## Caveat
Optimising $p_{\mathrm{succ}}$ is harder than optimising energy. It probes the low-energy tail, and the average success probability may be dominated by easy instances.

## Related notes
- [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]
