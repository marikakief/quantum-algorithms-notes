> **Source:** Gerardo Ortiz, J. E. Gubernatis, Emanuel Knill, and Raymond Laflamme, *Quantum algorithms for fermionic simulations*, Phys. Rev. A **64**, 022319 (2001)
> **Links:** [arXiv](https://arxiv.org/abs/cond-mat/0012334) · [PRA](https://doi.org/10.1103/PhysRevA.64.022319)
> **Tags:** #quantum-chemistry #jordan-wigner #fermionic #pauli-decomposition #foundational

---

## What the paper does

Gives explicit procedures for mapping fermionic Hamiltonians to qubit Hamiltonians via spin-particle mappings/generalized Jordan-Wigner transformations, and shows how to measure physical observables — charge density, correlations, momentum distributions — on a quantum computer. This is one of the papers that made Pauli decompositions of fermionic Hamiltonians concrete for simulation. Bravyi-Kitaev is a separate later encoding line, later adapted explicitly for chemistry by Seeley-Richard-Love.

---

## The Jordan-Wigner transformation

Maps fermionic creation/annihilation operators to qubit operators. With the occupation convention $|0\rangle=$ unoccupied, $|1\rangle=$ occupied, and $\sigma^+=|1\rangle\langle 0|$, $\sigma^-=|0\rangle\langle 1|$, one common Jordan-Wigner convention is:

$$
\hat{a}_j^\dagger \to Z_1\cdots Z_{j-1}\,\sigma_j^+,\qquad
\hat{a}_j \to Z_1\cdots Z_{j-1}\,\sigma_j^- .
$$

Some sources put the parity string on the opposite side depending on orbital ordering. The invariant content is that the string records the parity of modes preceding/following $j$ in the chosen ordering.

A second-quantized Hamiltonian $H = \sum_{pq} h_{pq} \hat{a}_p^\dagger \hat{a}_q + \sum_{pqrs} h_{pqrs} \hat{a}_p^\dagger \hat{a}_q^\dagger \hat{a}_r \hat{a}_s$ becomes a sum of tensor products of Pauli operators. The number of terms is $O(N^4)$ where $N$ is the number of spin orbitals — polynomial, but not necessarily cheap in modern measurement cost, since shot complexity and variance can dominate.

---

## Why this matters

This paper establishes the recipe used by many early and second-quantized quantum chemistry algorithms:
1. Start with the second-quantized Hamiltonian (integrals from Hartree-Fock)
2. Apply Jordan-Wigner → get a sum of Pauli tensor products
3. Simulate or measure each term on qubits

The decomposition into Pauli terms is what makes many [[Gapped Phase Estimation|phase-estimation-based]] and [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|variational]] approaches to second-quantized quantum chemistry possible. It is not the central representation for first-quantized plane-wave algorithms, real-space grid simulation, tensor-factorized qubitization, or other oracle-based chemistry methods.

---

## Reusable ideas

1. **[[Jordan-Wigner Transformation for Chemistry Hamiltonians]]:** Map fermionic operators to qubit Pauli operators. $O(N^4)$ terms for a molecular Hamiltonian. The $\sigma_z$ strings encode the fermionic antisymmetry.

2. **[[Basis Switching via QFT for Kinetic-Potential Splitting]]:** A separate first-quantized/grid simulation paradigm: simulate $H = T + V$ by switching between momentum space (where $T$ is diagonal) and position space (where $V$ is diagonal) via QFT.

---

## References within this paper

- Jordan & Wigner (1928) — original fermion-to-spin mapping
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — [[Hamiltonian simulation]] via [[Order-Condition Cancellation in Product Formulas|Trotter]]
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
