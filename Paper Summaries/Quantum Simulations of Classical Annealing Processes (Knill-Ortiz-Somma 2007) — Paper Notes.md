> **Source:** Emanuel Knill, Gerardo Ortiz, and Rolando D. Somma, *Optimal quantum measurements of expectation values of observables*, Phys. Rev. A **75**, 012328 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0607019) · [PRA](https://doi.org/10.1103/PhysRevA.75.012328)
> **Tags:** #measurement #expectation-values #hamiltonian-simulation #foundational

---

## What the paper does

Shows how to efficiently estimate expectation values $\langle\psi|H|\psi\rangle$ of a Hamiltonian $H = \sum_j h_j H_j$ (where each $H_j$ is efficiently simulable) by measuring each term separately and adding classically. This is the protocol that the [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE paper]] later packaged as "Algorithm 1" — but the idea originates here.

The paper gives the measurement cost:
$$
O\!\left(\frac{\|h\|_1^2}{p^2}\right) \text{ repetitions}
$$
for precision $p$ in the total expectation value, where $\|h\|_1 = \sum_j |h_j|$.

---

## The method

Given $H = \sum_{j=1}^M h_j H_j$ where each $H_j$ is unitary (or can be made so):

1. For each term $H_j$: prepare $|\psi\rangle$, apply a circuit that measures $\langle\psi|H_j|\psi\rangle$ (using the ability to simulate $e^{-iH_j t}$)
2. Estimate each $\langle H_j \rangle$ to precision $p/(\|h\|_1)$ using $O(\|h\|_1^2/p^2)$ shots total
3. Classically compute $\langle H \rangle = \sum_j h_j \langle H_j \rangle$

The key insight: you don't need to simulate the full Hamiltonian coherently. Each term is measured independently, and the linearity of expectation does the rest.

---

## Reusable ideas

1. **[[Term-by-Term Expectation Estimation]]:** Decompose $H$ into simulable terms, measure each independently, add classically. Cost $O(\|h\|_1^2/p^2)$. Trades coherence time (each measurement is short) for total measurement count. This is the actual origin of VQE's "Algorithm 1."

2. **[[Importance Sampling over Hamiltonian Terms]]:** Sample terms with probability $p_j = |h_j|/\|h\|_1$ instead of measuring uniformly. Reduces variance; cost depends on $\|h\|_1$ rather than $M \cdot \max |h_j|$.

---

## References within this paper

- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — [[Sparse Hamiltonian Simulation via Coloring Decomposition|sparse Hamiltonian simulation]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — [[Gapped Phase Estimation|phase estimation]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — phase estimation (via QFT) used in the eigenvalue sampling subroutine

---

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — repackages this protocol as "Algorithm 1"
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] — local Hamiltonian decomposition into Pauli terms

### Trick cards
- [[Term-by-Term Expectation Estimation]]
- [[Gapped Phase Estimation]]
