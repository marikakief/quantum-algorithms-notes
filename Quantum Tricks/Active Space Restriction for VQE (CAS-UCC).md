# Active Space Restriction for VQE (CAS-UCC)

> **Source:** Romero, Babbush, McClean, Hempel, Love, Aspuru-Guzik, arXiv:1701.02691 (2018)
> **Tags:** #trick #VQE #UCC #quantum-chemistry #active-space #qubit-reduction

## What it does

Reduces the qubit count and parameter scaling for [[Variational Quantum Eigensolver (VQE)|VQE]] with [[Unitary Coupled Cluster (UCC) Ansatz|UCC]] by restricting the quantum calculation to a chemically relevant active subspace of orbitals, replacing the rest with a classical mean-field (Hartree-Fock) approximation.

## The trick

Partition the $N$ spin-orbitals into:
- **Inactive** $I$: always doubly occupied (core/frozen orbitals). Treated at the Hartree-Fock level.
- **Active** $A$: $N_A$ orbitals with $\eta_A$ electrons — the strongly correlated part that needs quantum treatment.
- (Optionally) **Virtual** $V$: always empty, can be included in the active space if orbital rotations are allowed.

Restrict the cluster operator to active-space excitations only:

$$T_A = \sum_{i=1}^{\eta_A} T^{(i)}_A$$

where each $T^{(i)}_A$ involves excitations of $i$ electrons within the active space only.

The effective Hamiltonian acting on the active space is obtained by tracing out the inactive degrees of freedom:

$$\tilde{H}_A = \langle \Phi^I_0 | H | \Phi^I_0 \rangle$$

This is a standard "active-space integral" transformation — computed classically, it folds the mean-field contribution of inactive orbitals into renormalized one- and two-electron integrals.

Run VQE-UCCSD on $\tilde{H}_A$ using $N_A$ qubits:
- Qubit count: $N_A \ll N$
- Parameters: $O(\eta_A^2 N_A^2) \ll O(\eta^2 N^2)$

**Active space selection:** Various classical heuristics exist to choose which orbitals to include:
- Natural orbital occupancies from MP2: include orbitals with occupation $\notin \{0, 2\}$ (fractionally occupied = correlated).
- Unrestricted Hartree-Fock natural orbitals (UNOs): good for spin-broken systems.
- Entanglement-based selection from a small DMRG calculation: most systematic.
- Chemical intuition: for bond breaking, include the bonding/antibonding pair.

## When to reach for it

- Any time the full-molecule qubit count or circuit depth exceeds what the hardware supports, but you can identify a smaller strongly correlated subspace.
- For large organic molecules where only a few orbitals (typically a $\pi$ system or metal $d$ orbitals) drive the chemistry.
- As a systematic way to scale [[Variational Quantum Eigensolver (VQE)|VQE]] to larger molecules before fault tolerance: choose the active space based on available qubit count, then refine classically.
- Standard in quantum chemistry (CASSCF, CASPT2, NEVPT2) — the quantum device replaces the CASSCF step, and a classical perturbation theory correction can be applied afterward.

## Complexity

- **Qubits:** $N_A$ (active orbitals) vs $N$ (full molecule). Can reduce from $N \sim 100$ to $N_A \sim 10$–$20$ for typical drug-like molecules.
- **Parameters:** $O(\eta_A^2 N_A^2)$, independent of the full molecular size $N$.
- **Classical preprocessing:** Active-space integral transformation is $O(N^5)$ — comparable to MP2. Done once, not per VQE iteration.
- **Accuracy:** Controlled by the active space size. Error relative to full UCCSD decreases as $N_A \to N$. For well-chosen active spaces, often achieves chemical accuracy even with $N_A \ll N$.

## Caveat

- **Active space selection is non-trivial and non-systematic:** Poor orbital selection can miss important correlations entirely. There is no universal rule for which orbitals to include; it requires chemical knowledge or expensive classical preprocessing.
- **Inactive orbitals at mean-field level:** Core correlations are ignored. For properties sensitive to core electrons (NMR shifts, core excitations, relativistic effects), the active-space approximation may be too crude.
- **Not a full solution:** CAS-UCC gives the FCI energy within the active space, but the active-space-to-full-space extrapolation is a classical problem. Post-CASSCF corrections (NEVPT2, CASPT2) are often needed for quantitative accuracy.
- **Qubit count reduction is not the same as gate depth reduction:** The circuit depth for UCC within the active space may still be large if $N_A$ is not tiny.

## Related notes
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]]
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — VQSE and orbital relaxation recover accuracy lost by the active-space restriction
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Virtual Quantum Subspace Expansion (VQSE)]] — corrects basis incompleteness from CAS-UCC
- [[Orbital Relaxation via RDM Postprocessing]] — cheap postprocessing to partially compensate for active-space limitation
- [[MP2-Based Amplitude Prescreening for UCC]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
