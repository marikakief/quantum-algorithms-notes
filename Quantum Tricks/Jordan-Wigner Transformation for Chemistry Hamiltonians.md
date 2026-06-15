
> **Source:** Jordan & Wigner (1928); made explicit for quantum computing by Ortiz, Gubernatis, Knill & Laflamme, PRA 64, 022319 (2001)
> **Tags:** #trick #fermionic #jordan-wigner #quantum-chemistry

## What it does

Maps fermionic creation/annihilation operators to tensor products of Pauli operators on qubits. This transforms a second-quantized chemistry Hamiltonian into a sum of Pauli terms that a quantum computer can simulate or measure.

## The mapping

Fix a mode ordering and the occupation convention $|0\rangle=$ unoccupied, $|1\rangle=$ occupied. Let $\sigma^+=|1\rangle\langle 0|=(X-iY)/2$ and $\sigma^-=|0\rangle\langle 1|=(X+iY)/2$. One common convention is:

$$
\hat{a}_j^\dagger \to Z_1\cdots Z_{j-1}\,\sigma_j^+,\qquad
\hat{a}_j \to Z_1\cdots Z_{j-1}\,\sigma_j^- .
$$

Some authors put the $Z$ string on the other side, depending on whether the orbital ordering is read left-to-right or right-to-left. The invariant content is that the string records the parity of occupied modes before/after $j$ in the chosen ordering.

The $\sigma_z$ string encodes the fermionic parity — it ensures the correct anticommutation relations $\{\hat{a}_i, \hat{a}_j^\dagger\} = \delta_{ij}$.

A molecular Hamiltonian $H = \sum_{pq} h_{pq} \hat{a}_p^\dagger \hat{a}_q + \sum_{pqrs} h_{pqrs} \hat{a}_p^\dagger \hat{a}_q^\dagger \hat{a}_r \hat{a}_s$ becomes a sum of $O(N^4)$ Pauli tensor products.

## When to reach for it

- Many second-quantized quantum chemistry calculations, especially early Trotter/QPE and VQE implementations
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]] and [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|QPE-based chemistry]] when the Hamiltonian is represented as Pauli strings
- [[Term-by-Term Expectation Estimation|Expectation estimation]] of chemistry Hamiltonians

## Complexity

- $N$ spin orbitals → up to $O(N^4)$ Pauli terms for a generic molecular Hamiltonian before exploiting symmetries, sparsity, low-rank structure, or basis-specific simplifications
- Each Pauli string has weight up to $O(N)$ (the $\sigma_z$ strings)
- Pauli weight is distinct from circuit depth: implementation cost also depends on connectivity, routing, and whether parity is computed by CNOT ladders, swap networks, or another compilation strategy

## Caveat

The $\sigma_z$ strings make the Pauli terms nonlocal even for spatially local interactions. Alternative mappings (Bravyi-Kitaev, ternary tree) reduce the string length to $O(\log N)$ at the cost of more complex structure. Many modern chemistry algorithms instead use first quantization, plane waves, dual bases, low-rank or tensor-factorized Hamiltonians, or block-encoding oracles where Jordan-Wigner is not the central representation. For second-quantized simulation, see also [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|sublinear block-encoding]] approaches that change the input/oracle model rather than eliminating all Hamiltonian input-size costs.

## Related notes

- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
- [[Fermionic-to-Qubit Mapping for Simplicial Dirac Operators]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — compares JW vs BK gate cost for UCC; JW gives $O(N)$ gates/parameter vs $O(\log N)$ for BK
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — uses BK (not JW) for H₂ simulation; demonstrates why the JW $O(N)$ string length motivates alternative mappings
