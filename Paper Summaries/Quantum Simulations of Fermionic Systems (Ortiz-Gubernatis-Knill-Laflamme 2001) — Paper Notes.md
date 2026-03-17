> **Source:** Gerardo Ortiz, J. E. Gubernatis, Emanuel Knill, and Raymond Laflamme, *Quantum algorithms for fermionic simulations*, Phys. Rev. A **64**, 022319 (2001)
> **Links:** [arXiv](https://arxiv.org/abs/cond-mat/0012334) · [PRA](https://doi.org/10.1103/PhysRevA.64.022319)
> **Tags:** #quantum-chemistry #jordan-wigner #fermionic #pauli-decomposition #foundational

---

## What the paper does

Gives explicit procedures for mapping fermionic Hamiltonians to qubit Hamiltonians (via Jordan-Wigner and Bravyi-Kitaev transformations) and shows how to measure physical observables — charge density, correlations, momentum distributions — on a quantum computer. This is the paper that made the Pauli decomposition of chemistry Hamiltonians concrete and practical.

---

## The Jordan-Wigner transformation

Maps fermionic creation/annihilation operators to qubit operators:

$$
\hat{a}_j \to I^{\otimes(j-1)} \otimes \sigma_+ \otimes \sigma_z^{\otimes(N-j)}, \qquad \hat{a}_j^\dagger \to I^{\otimes(j-1)} \otimes \sigma_- \otimes \sigma_z^{\otimes(N-j)}
$$

A second-quantized Hamiltonian $H = \sum_{pq} h_{pq} \hat{a}_p^\dagger \hat{a}_q + \sum_{pqrs} h_{pqrs} \hat{a}_p^\dagger \hat{a}_q^\dagger \hat{a}_r \hat{a}_s$ becomes a sum of tensor products of Pauli operators. The number of terms is $O(N^4)$ where $N$ is the number of spin orbitals — polynomial, so [[Term-by-Term Expectation Estimation|term-by-term measurement]] is efficient.

---

## Why this matters

This paper establishes the recipe that all quantum chemistry algorithms use:
1. Start with the second-quantized Hamiltonian (integrals from Hartree-Fock)
2. Apply Jordan-Wigner → get a sum of Pauli tensor products
3. Simulate or measure each term on qubits

The decomposition into Pauli terms is what makes both [[Gapped Phase Estimation|phase-estimation-based]] and [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|variational]] approaches to quantum chemistry possible.

---

## Reusable ideas

1. **[[Jordan-Wigner Transformation for Chemistry Hamiltonians]]:** Map fermionic operators to qubit Pauli operators. $O(N^4)$ terms for a molecular Hamiltonian. The $\sigma_z$ strings encode the fermionic antisymmetry.

2. **[[Basis Switching via QFT for Kinetic-Potential Splitting]]:** Simulate $H = T + V$ by switching between momentum space (where $T$ is diagonal) and position space (where $V$ is diagonal) via QFT. Enables cheap per-step Trotter simulation of first-quantized systems.

---

## References within this paper

- Jordan & Wigner (1928) — original fermion-to-spin mapping
- Lloyd (1996) — Hamiltonian simulation via [[Order-Condition Cancellation in Product Formulas|Trotter]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams & Lloyd (1999)]] — eigenvalue algorithm that this paper provides the Hamiltonian encoding for

---

## Cross-links

### Paper notes
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] — improves fermionic state preparation using [[Sorting Networks as Quantum Control-Flow Compilers|sorting networks]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — uses the Pauli decomposition from this paper
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]] — the eigenvalue algorithm these Hamiltonians are fed into

### Trick cards
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Fermionic-to-Qubit Mapping for Simplicial Dirac Operators]]
- [[Term-by-Term Expectation Estimation]]
