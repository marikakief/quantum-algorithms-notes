# Compressed UCC as ADAPT-VQE Initial State

> **Source:** Rubin, Lee, Babbush, arXiv:2109.05010 (2021)
> **Tags:** #trick #state-preparation #ADAPT-VQE #UCC #initial-state #overlap

## What it does

Uses a small number of [[Greedy Sum-of-Squares Operator Decomposition|unitary compression]] factors to approximate a CCSD wavefunction, then feeds this as the starting state for ADAPT-VQE or other iterative circuit construction methods. Gives ADAPT a fighting chance on systems where Hartree-Fock has negligible overlap with the ground state.

## The trick

ADAPT-VQE (Grimsley et al. 2019) builds a circuit iteratively: at each step, it picks the operator from a pool with the largest energy gradient, appends it to the circuit, and re-optimizes all parameters. The gradient-based selection means ADAPT can get stuck if the initial state has near-zero overlap with the target — all gradients are tiny and the optimizer has no useful signal.

The fix: prepare an approximate CCSD state using [[Greedy Sum-of-Squares Operator Decomposition|unitary compression]] of the $\tau_2 = T_2 - T_2^\dagger$ doubles generator. Even a coarse approximation (6 tensor factors for triplet O₂ in STO-3G) captures enough correlation energy to give ADAPT a good starting gradient landscape.

**Circuit structure of the initial state:**

$$|\psi_0\rangle = \left(\prod_{l=1}^{L} U_l \, e^{\sum_{pq} J^{(l)}_{pq} n_p n_q} \, U_l^\dagger \right) e^{T_1 - T_1^\dagger} |\phi_{\text{HF}}\rangle$$

where each $U_l$ is a [[Givens Rotation Slater Determinant Preparation|Givens rotation network]] and the Ising layer uses a [[Fermionic Swap Network|swap network]]. Singles are exact (they're one-body operators). The compressed doubles require $L$ interleaved Givens + Ising layers.

**Empirical result on triplet O₂ (STO-3G, 10 orbitals, 16 electrons):**

| Starting state | FCI overlap | ADAPT converges? |
|---|---|---|
| ROHF | $< 10^{-5}$ | No |
| UHF | ~0.35 | Yes (with $S^2$ symmetry breaking) |
| Compressed RO-CCSD (6 factors) | ~CCSD-level | Yes (17 iterations for 1 mHa of 2-uCJ) |

The ROHF failure is instructive: even an adaptive, gradient-driven method can't escape a region of exponentially small overlap. The compressed CCSD state costs ~6 Givens+Ising layers but enables convergence.

## When to reach for it

- Systems where the Hartree-Fock reference has poor overlap with the ground state (strongly correlated, multi-reference character).
- When ADAPT-VQE or similar iterative methods (AQCC, quantum contracted Schrödinger equation) fail from a mean-field starting point.
- When you need a better-than-HF initial state for [[Variational Quantum Eigensolver (VQE)|VQE]] but can't afford the full UCCSD circuit.
- As a warm start before any iterative quantum algorithm that's sensitive to initial overlap (this includes QPE in the sense of [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|the orthogonality catastrophe]]).

## Complexity

- **Classical preprocessing:** $O(Ln^5)$ for $L$ greedy iterations of unitary compression on the CCSD doubles tensor.
- **Quantum circuit:** $L$ layers of $O(n)$-depth Givens + $O(n)$-depth Ising = $O(Ln)$ total depth. For the O₂ example, $L = 6$, so 6 × (Givens + Ising) ≈ 12 linear-depth layers before ADAPT begins.
- **Comparison:** Full Takagi-based CCSD compilation needs ~25 factors for the same accuracy → $4\times$ deeper circuit.

## Caveat

This is a near-term / NISQ technique. The compressed state is approximate (first-order Trotter of an approximate decomposition), so it's not suitable as a precision initial state for fault-tolerant QPE where you need certified overlap bounds. For that, see [[ASCI Overlap Estimation for QPE Initial State]] or [[Sequential Multi-Determinant State Preparation]].

Also, CCSD itself can fail for strongly multi-reference systems. If classical CCSD doesn't converge or gives nonsense, compressing it won't help.

## Related notes
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
- [[Greedy Sum-of-Squares Operator Decomposition]]
- [[Full-Rank Ising Coupling via Numerical Basis Optimization]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]]
- [[ASCI Overlap Estimation for QPE Initial State]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Fermionic Swap Network]]
