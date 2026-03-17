> **Source:** Yi-Kai Liu, Matthias Christandl, and Frank Verstraete, *Quantum computational complexity of the N-representability problem: QMA complete*, Phys. Rev. Lett. **98**, 110503 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0609125) · [PRL](https://doi.org/10.1103/PhysRevLett.98.110503)
> **Tags:** #QMA #N-representability #complexity #quantum-chemistry

---

## What the paper does

Proves that the $N$-representability problem is QMA-complete. This is the problem of deciding whether a given 2-particle reduced density matrix (2-RDM) is consistent with some $N$-particle quantum state.

This result has a direct implication for quantum chemistry: **you cannot efficiently optimize over 2-RDMs to find the ground state energy**, even with a quantum computer (unless QMA = BQP). This is why the [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]] paper correctly states that its approach sidesteps the $N$-representability problem by working with a global quantum state rather than reduced density matrices.

---

## The problem

**$N$-representability:** Given a 2-body reduced density matrix $\rho_{12}$, does there exist an $N$-particle state $|\Psi\rangle$ such that $\mathrm{Tr}_{3,\ldots,N} |\Psi\rangle\langle\Psi| = \rho_{12}$?

This is the central obstacle for classical "reduced density matrix" methods in quantum chemistry. If you could efficiently check $N$-representability, you could optimize the energy $\mathrm{Tr}(H_{12} \rho_{12})$ over all valid 2-RDMs and find the ground state energy. The QMA-completeness kills this approach for both classical and quantum computers.

---

## The reduction

The proof reduces the [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|2-local Hamiltonian problem]] to $N$-representability. Since a 2-local Hamiltonian $H = \sum_{ij} H_{ij}$ has energy $\langle\Psi|H|\Psi\rangle = \sum_{ij} \mathrm{Tr}(H_{ij} \rho_{ij})$, the ground energy is determined by the 2-RDMs. So deciding ground energy → deciding $N$-representability.

---

## Why this matters for quantum algorithms

This result draws a sharp line:
- **Global state approaches** (QPE, VQE, [[QSVT Meta-Template|QSVT]]-based preparation): work with the full $N$-particle state, avoid $N$-representability entirely
- **RDM-based approaches** (classical 2-RDM optimization): blocked by QMA-hardness

The VQE paper cites this result as justification for working with a global quantum state. But note: VQE still has no provable guarantee of finding the ground state, because the optimization landscape can have barren plateaus and local minima.

---

## Reusable ideas

This is primarily a complexity result rather than an algorithmic technique. No new trick cards.

---

## References within this paper

- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe, Kitaev & Regev (2006)]] — 2-local Hamiltonian is QMA-complete (the reduction source)
- Coleman (1963) — original formulation of the $N$-representability problem
- Mazziotti (2004) — variational 2-RDM methods in quantum chemistry

---

## Cross-links

### Paper notes
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]] — the underlying QMA-completeness result
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — cites this result to justify working with global states
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] — the QMA-completeness lineage

### Trick cards
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
