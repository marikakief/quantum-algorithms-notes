# Virtual Correlation via Matchgate Contraction

> **Source:** Huggins, O'Gorman, Rubin, Reichman, Babbush, Lee, arXiv:2106.16235
> **Tags:** #trick #matchgate #active-space #virtual-correlation #QMC #AFQMC

## What it does

Reduces a full-orbital-space wavefunction overlap to an active-space overlap, allowing a quantum computer with $N_A$ qubits to unbias a classical QMC calculation running over $N \gg N_A$ orbitals — with zero additional quantum resources or measurements.

## The trick

Suppose the trial wavefunction factorizes as $|\Psi_T\rangle = |\psi_T\rangle \otimes |\psi_c\rangle \otimes |0_v\rangle$, where $|\psi_T\rangle$ is the active-space state (on the quantum device), $|\psi_c\rangle$ is the frozen-core Slater determinant, and $|0_v\rangle$ is the virtual-orbital vacuum. The overlap with a full-space walker $|\phi\rangle$ is:

$$\langle \phi | \Psi_T \rangle = \sum_{x \in \{0,1\}^{N_A}} \left(\sum_{y \in \{0,1\}^{N_c}} \phi^*(x,y,0_v)\,\psi_c(y)\right) \psi_T(x)$$

The inner sum contracts a matchgate tensor (the Slater determinant $|\phi\rangle$) over the core and virtual indices against another matchgate ($|\psi_c\rangle \otimes |0_v\rangle$). By Bravyi's matchgate contraction result, the output is itself a matchgate tensor with $N_A$ open indices — i.e., a Slater determinant $|\tilde{\phi}\rangle$ in the active space. So:

$$\langle \phi | \Psi_T \rangle = \text{const} \times \langle \tilde{\phi} | \psi_T \rangle$$

The constant is computed classically (determinant of a submatrix). The active-space determinant $|\tilde{\phi}\rangle$ is formed by $O(N^3)$ classical linear algebra (Gaussian elimination on the orbital coefficient matrix).

The same reduction applies to the excited determinants $|\phi^r_p\rangle$, $|\phi^{rs}_{pq}\rangle$ needed for local energy evaluation, so the full QC-AFQMC algorithm (constraint + energy estimation) runs on $N_A$ qubits while the classical AFQMC explores the $N$-orbital Hilbert space.

## When to reach for it

- Any hybrid quantum-classical algorithm where the quantum part acts on an active space and you want to include correlation from inactive orbitals.
- Preferable to [[Virtual Quantum Subspace Expansion (VQSE)|VQSE]] when you want to avoid the measurement overhead of 3- and 4-RDMs.
- Applicable whenever the trial wavefunction factorizes cleanly between active and inactive spaces (no inter-space entanglement in the trial).

## Complexity

- **Classical cost:** $O(N_A \cdot N^2)$ per walker per time step for the matchgate contraction (dominated by the determinant computation).
- **Quantum cost:** Zero additional cost beyond the active-space calculation.
- **Limitation:** Requires the trial to factorize as active $\otimes$ core $\otimes$ vacuum. Trials with active-inactive entanglement don't admit this reduction.

## Caveat

The factorization assumption means the quantum trial only captures correlation within the active space. The virtual correlation energy comes from the AFQMC propagation exploring the full space, not from the trial itself. If the true ground state has strong active-virtual entanglement that the trial completely misses, the constraint quality suffers. The technique is most effective when the active space captures the static correlation and the virtual orbitals contribute mostly dynamic correlation.

## Related notes

- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]]
- [[Virtual Quantum Subspace Expansion (VQSE)]] — alternative virtual correlation method with higher measurement cost
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — VQSE paper, the direct comparison
- [[Active Space Restriction for VQE (CAS-UCC)]] — the active-space framework that motivates this trick
- [[Shadow Tomography for QMC Overlap Estimation]] — the measurement protocol this trick enables
