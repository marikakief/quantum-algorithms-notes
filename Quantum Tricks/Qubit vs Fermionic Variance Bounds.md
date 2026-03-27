
> **Source:** Huggins, McClean, Rubin, Jiang, Wiebe, Whaley, Babbush, arXiv:1907.13117
> **Tags:** #trick #measurement #bounds #Jordan-Wigner #quantum-chemistry #cost-estimation

## What it does

Shows that computing measurement-cost bounds from the Jordan-Wigner (qubit) representation of a molecular Hamiltonian gives substantially tighter estimates than computing them from the fermionic (second-quantized) representation — by a factor of $\sim 3\times$ in $\Lambda^2$ (hence $\sim 3\times$ fewer predicted shots) for medium molecular systems.

## The trick

The standard measurement bound for Hamiltonian averaging is:

$$M \leq \left(\frac{\Lambda}{\varepsilon}\right)^2, \qquad \Lambda = \sum_\ell |w_\ell|$$

where $w_\ell$ are the coefficients in the operator decomposition $H = \sum_\ell w_\ell O_\ell$.

If you evaluate $\Lambda$ using the fermionic coefficients $h_{pq}$, $h_{pqrs}$ directly, you get a bound $M_f$. If you first apply the Jordan-Wigner transformation and compute $\Lambda$ from the resulting Pauli string coefficients, you get $M_q$. Naively these should be similar, since JW is a linear map. But in practice $M_q \ll M_f$, for three reasons:

1. **Number operators have reduced variance.** $a_p^\dagger a_p$ maps to $(I + Z_p)/2$; the constant $I/2$ term has zero variance. This halves the contribution of diagonal one-body terms and density-density two-body terms ($a_p^\dagger a_p a_q^\dagger a_q$).

2. **Eight-fold symmetry cancellation.** For the class V two-body terms (4 distinct orbital indices), the eight-fold permutation symmetry of real two-electron integrals $h_{pqrs} = h_{qpsr} = \ldots$ causes JW Pauli terms from different symmetry-related fermionic terms to partially cancel. The net effect: the qubit $\Lambda$ for these terms is exactly half the fermionic $\Lambda$.

3. **Cross-class cancellation.** When summing $\Lambda$ over all term types in the qubit representation, additional cancellation occurs between single-$Z$ contributions from one-body terms (negative, from nuclear attraction) and $ZZ$ contributions from two-body terms (positive, from electron repulsion). This is a physical effect tied to the opposite signs of nuclear-electron and electron-electron Coulomb interactions.

For an H₈ chain (STO-3G), the qubit bound is $\Lambda_q^2 \approx 33.5$ vs the fermionic bound $\Lambda_f^2 \approx 112.4$ — a $3.4\times$ difference.

**Practical implication:** Resource estimates for quantum chemistry algorithms (including fault-tolerant ones) that use $\Lambda_f$ to bound measurement or simulation costs may be overly pessimistic. Recalculating using $\Lambda_q$ can tighten estimates significantly.

## When to reach for it

- When estimating the measurement cost of any VQE or QPE-based chemistry algorithm.
- When comparing resource estimates across papers — check whether they used fermionic or qubit bounds.
- When optimizing Hamiltonian coefficients via LP (as in [[Variance Reduction via N-Representability Constraints (LP Method)|Rubin, Babbush, McClean (2018)]]) — the optimization should be performed on the qubit representation.
- When estimating the 1-norm $\lambda$ of an LCU decomposition for fault-tolerant algorithms.

## Complexity

No additional cost — this is about choosing which representation to analyze, not a new algorithm.

## Caveat

- The cancellation effects are specific to real-orbital molecular Hamiltonians with the standard Coulomb form. For model Hamiltonians (e.g., Hubbard) or complex orbitals, the gap may be smaller or absent.
- This affects bounds and cost estimates, not the actual algorithm. The actual variance with a specific measurement strategy may differ from these bounds.
- The paper found that applying [[Variance Reduction via N-Representability Constraints (LP Method)|RDM constraints]] in the qubit representation (instead of fermionic) does not substantially improve the *actual* variance — the tighter bound doesn't translate to a tighter variance. The gap between the bound and reality is smaller in the qubit representation.

## Related notes
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]]
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[Pauli Expectation Value Estimation]]
- [[Bravyi-Kitaev Transformation]]
