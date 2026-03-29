> **Source:** Julia Kempe, Alexei Kitaev, and Oded Regev, *The Complexity of the Local Hamiltonian Problem*, SIAM J. Comput. **35**(5), 1070–1097 (2006)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0406180) · [SICOMP](https://doi.org/10.1137/S0097539704445226)
> **Tags:** #QMA #local-hamiltonian #2-local #perturbation #complexity

---

## What the paper does

Settles the complexity of the 2-local Hamiltonian problem: it is QMA-complete. This is tight — 1-local Hamiltonian is in P. So $k = 2$ is the threshold for quantum hardness, mirroring the classical situation where 2-SAT is in P but MAX-2-SAT is NP-complete.

Two independent proofs are given. The second uses a perturbation-theory technique for analyzing sums of Hamiltonians that became widely influential.

---

## The result

**Theorem:** The 2-local Hamiltonian problem is QMA-complete, even with the promise gap $b - a \geq 1/\mathrm{poly}(n)$.

This implies: even with a quantum computer, determining whether the ground energy of a 2-local Hamiltonian is below $a$ or above $b$ requires QMA resources. No polynomial quantum algorithm can solve it in general (unless QMA = BQP).

**Consequence for VQE:** The [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE algorithm]] tries to solve instances of this problem heuristically. The QMA-completeness result means there can be no efficient algorithm with provable worst-case guarantees — not even a quantum one.

---

## The perturbation technique

The paper introduces a refined perturbation gadget construction for reducing $k$-local to $(k-1)$-local Hamiltonians. The idea: replace a $k$-body interaction with auxiliary "mediator" qubits that interact pairwise, and show that the low-energy effective Hamiltonian of the gadget reproduces the original $k$-body term.

This is more sophisticated than the [[Perturbation Lemma for Locality Reduction|Kempe-Regev penalty technique]] — it constructs explicit 2-local Hamiltonians whose ground-space physics matches the target $k$-local Hamiltonian via perturbation theory.

---

## Reusable ideas

1. **[[Perturbation Gadgets for Locality Reduction]]:** General framework for reducing $k$-local interactions to 2-local using auxiliary qubits and energy penalties. More refined than [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]].

2. **[[Subdivision Gadget for k-to-2-Local Reduction]]:** The specific construction: chain mediator qubits between the original qubits, use second-order perturbation theory to recover the $k$-body effective interaction.

---

## References within this paper

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999/2002)]] — 5-local QMA-completeness
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe & Regev (2003)]] — 3-local QMA-completeness
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — uses these techniques for adiabatic equivalence (Theorem 1.3: 2-local nearest-neighbor on 2D grid)

---

## Cross-links

### Paper notes
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — attempts to solve instances of this problem
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]] — generalises this result to a full complexity classification of all 2-qubit Hamiltonians
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]] — builds on this and Cubitt-Montanaro 2016 to construct universal Hamiltonians

### Trick cards
- [[Perturbation Gadgets for Locality Reduction]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Kitaev Geometric Lemma for Ground Space Angles]]
- [[Mediator Qubit Gadget for Interaction Generation]]
