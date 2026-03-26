# Virtual Quantum Subspace Expansion (VQSE)

> **Source:** Takeshita, Rubin, Jiang, Lee, Babbush, McClean, arXiv:1902.10679
> **Tags:** #trick #subspace-expansion #active-space #NISQ #quantum-chemistry #RDM #dynamic-correlation

## What it does

Recovers dynamic correlation from virtual (unoccupied) orbitals without adding qubits or circuit depth to an active-space quantum calculation. Trades quantum resources for additional measurements and classical postprocessing.

## The trick

Start with a Quantum Subspace Expansion (QSE) setup: given a reference state $|\Psi_\text{ref}\rangle$ prepared on the quantum device in an active space $A$, form expansion operators $\{O_i\}$ and build the projected Hamiltonian and overlap matrices:

$$H_{ij} = \langle \Psi_\text{ref}| O_i^\dagger H\, O_j |\Psi_\text{ref}\rangle, \qquad S_{ij} = \langle \Psi_\text{ref}| O_i^\dagger O_j |\Psi_\text{ref}\rangle$$

**Standard QSE** restricts $\{O_i\}$ to active-space excitations. **VQSE** includes excitations into virtual orbitals — operators like $a_\mu^\dagger a_p$ and $a_\mu^\dagger a_q\, a_\nu^\dagger a_r$ where $\mu, \nu \in V$ (virtual) and $p, q, r \in A$ (active).

The matrix elements now contain virtual-orbital operators acting on $|\Psi_\text{ref}\rangle$. But $|\Psi_\text{ref}\rangle$ has zero virtual occupancy, so all virtual operator pairs contract via Wick's theorem to Kronecker deltas. What remains are expectation values of active-space operators only — i.e., elements of the active-space RDMs.

The full-basis molecular integrals (computed classically) multiply the contracted RDM elements to yield the VQSE matrix elements. Solve $HC = SCE$ classically.

**Measurement requirement:** For singles + doubles expansion, the 4-RDM of the active space is needed. Measurement cost scales as $\sim N_A^8$ (naive count of distinct 4-RDM elements). [[Cumulant Truncation for RDM Measurement Reduction|Cumulant truncation]] can reduce this to $\sim N_A^4$ at the cost of approximation.

## When to reach for it

- You're running [[Variational Quantum Eigensolver (VQE)|VQE]] with an [[Active Space Restriction for VQE (CAS-UCC)|active-space restriction]] and the basis incompleteness error is too large for your target accuracy.
- Your qubit budget is fixed (hardware-limited) but you can afford more measurement shots.
- You want excited-state estimates as a bonus — the generalized eigenvalue problem yields multiple eigenvalues.
- You're comparing against classical MRCI or CASPT2 and want the quantum analogue that exploits the exponentially expressive reference wavefunction.

## Complexity

- **Qubits / circuit depth:** Unchanged from the base active-space calculation.
- **Measurements:** $\sim N_A^{2(2+k)}$ for excitations of level $k$ from the active space. Singles + doubles: $\sim N_A^8$.
- **Classical postprocessing:** Form the VQSE matrices (size grows with $N_V \cdot N_A$ for singles, $N_V^2 \cdot N_A^2$ for doubles) and solve a generalized eigenvalue problem. Polynomial in $N_V$.

## Caveat

- The 4-RDM measurement overhead is the practical bottleneck. For active spaces beyond $\sim 20$ orbitals, this becomes expensive even with grouping and cumulant approximations.
- Accuracy of the Wick contraction depends on having a good active-space reference. If the active space misses important static correlations, VQSE can't compensate.
- On noisy hardware, RDM estimation errors propagate through the Wick contractions and can degrade the generalized eigenvalue solve. Combining with [[2-RDM Physicality Restoration by PSD Projection|RDM physicality restoration]] is advisable.
- The generalized eigenvalue problem can be ill-conditioned when the overlap matrix $S$ is near-singular (linearly dependent expansion vectors). Regularization or truncation of small singular values is needed.

## Related notes
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]]
- [[Active Space Restriction for VQE (CAS-UCC)]] — the approximation VQSE corrects
- [[Variational Quantum Eigensolver (VQE)]] — produces the reference state
- [[Orbital Relaxation via RDM Postprocessing]] — complementary technique from the same paper
- [[Cumulant Truncation for RDM Measurement Reduction]] — reduces VQSE measurement cost
- [[Variance Reduction via N-Representability Constraints (LP Method)]] — reduces shot count for the RDMs VQSE needs
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — alternative virtual correlation approach via [[Virtual Correlation via Matchgate Contraction|matchgate contraction]] that avoids VQSE's RDM measurement overhead
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]] — same QSE framework applied to error mitigation via stabilizer projection
- [[Symmetry-Based QSE for Unencoded Error Mitigation]] — error mitigation variant of QSE
