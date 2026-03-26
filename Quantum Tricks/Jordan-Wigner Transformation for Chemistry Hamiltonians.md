# Jordan-Wigner Transformation for Chemistry Hamiltonians

> **Source:** Jordan & Wigner (1928); made explicit for quantum computing by Ortiz, Gubernatis, Knill & Laflamme, PRA 64, 022319 (2001)
> **Tags:** #trick #fermionic #jordan-wigner #quantum-chemistry

## What it does

Maps fermionic creation/annihilation operators to tensor products of Pauli operators on qubits. This transforms a second-quantized chemistry Hamiltonian into a sum of Pauli terms that a quantum computer can simulate or measure.

## The mapping

$$
\hat{a}_j \to I^{\otimes(j-1)} \otimes \sigma_+ \otimes \sigma_z^{\otimes(N-j)}
$$

$$
\hat{a}_j^\dagger \to I^{\otimes(j-1)} \otimes \sigma_- \otimes \sigma_z^{\otimes(N-j)}
$$

The $\sigma_z$ string encodes the fermionic parity — it ensures the correct anticommutation relations $\{\hat{a}_i, \hat{a}_j^\dagger\} = \delta_{ij}$.

A molecular Hamiltonian $H = \sum_{pq} h_{pq} \hat{a}_p^\dagger \hat{a}_q + \sum_{pqrs} h_{pqrs} \hat{a}_p^\dagger \hat{a}_q^\dagger \hat{a}_r \hat{a}_s$ becomes a sum of $O(N^4)$ Pauli tensor products.

## When to reach for it

- Any quantum chemistry calculation: this is step 1 of mapping the problem to qubits
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]], [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|QPE-based chemistry]], and fault-tolerant approaches all use this
- [[Term-by-Term Expectation Estimation|Expectation estimation]] of chemistry Hamiltonians

## Complexity

- $N$ spin orbitals → $O(N^4)$ Pauli terms (two-electron integrals dominate)
- Each Pauli string has weight up to $O(N)$ (the $\sigma_z$ strings)
- The string length is the main weakness: makes simulation cost scale with $N$ even for local interactions

## Caveat

The $\sigma_z$ strings make the Pauli terms nonlocal even for spatially local interactions. Alternative mappings (Bravyi-Kitaev, ternary tree) reduce the string length to $O(\log N)$ at the cost of more complex structure. For second-quantized simulation, see also [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|sublinear block-encoding]] approaches that avoid the full $N^4$ term count.

## Related notes

- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
- [[Fermionic-to-Qubit Mapping for Simplicial Dirac Operators]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — compares JW vs BK gate cost for UCC; JW gives $O(N)$ gates/parameter vs $O(\log N)$ for BK
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — uses BK (not JW) for H₂ simulation; demonstrates why the JW $O(N)$ string length motivates alternative mappings
