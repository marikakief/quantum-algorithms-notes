> **Source:** Nicholas C. Rubin, Joonho Lee, Ryan Babbush, *Compressing Many-Body Fermion Operators Under Unitary Constraints*, arXiv:2109.05010, J. Chem. Theory Comput. (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2109.05010) · [Journal](https://doi.org/10.1021/acs.jctc.1c00912)
> **Tags:** #quantum-chemistry #circuit-compilation #UCC #operator-decomposition #sum-of-squares #NISQ #fermionic-Gaussian #ADAPT-VQE

---

## The computational problem

Given a generic antihermitian two-body fermion operator

$$G = \sum_{pqrs} A^{pq}_{rs}\, a^\dagger_p a^\dagger_q a_s a_r$$

decompose it into a sum-of-squares of normal one-body operators

$$G = \sum_l Z_l^2 - \sum_{pr} S_{pr}\, a^\dagger_p a_r, \quad Z_l = \sum_{pq} z^{(l)}_{pq}\, a^\dagger_p a_q$$

such that $L$ (the number of terms) is minimized while the reconstruction error stays below a target threshold. The decomposition determines efficient quantum circuits: each $Z_l^2$ term maps to a fermionic Gaussian unitary (basis rotation) sandwiching Ising-type $n_p n_q$ interactions, both implementable in $O(n)$ depth on a linear qubit chain via [[Givens Rotation Slater Determinant Preparation|Givens rotation networks]] and [[Fermionic Swap Network|swap networks]].

## What the paper does

Introduces **unitary compression**, a greedy numerical algorithm that recursively finds single-particle basis rotations maximizing the diagonal ($n_i n_j$) content of a two-body operator, extracts those components, and repeats on the residual. Each iteration costs $O(n^5)$ — the same as a single one-particle basis transformation — and the resulting decomposition typically needs far fewer terms than the analytical alternatives (SVD or Takagi decomposition of the coefficient supermatrix). For [[Unitary Coupled Cluster (UCC) Ansatz|unitary coupled cluster doubles]] operators, the compression reaches sub-milliHartree correlation energy error with roughly $4\times$ fewer tensor factors than Takagi.

The paper also demonstrates that compressed UCC is an effective starting state for [[Variational Quantum Eigensolver (VQE)|ADAPT-VQE]], showing that ADAPT fails to converge for triplet O₂ from a restricted Hartree-Fock reference (overlap $< 10^{-5}$ with FCI) but succeeds from a 6-factor compressed CCSD initial state.

My assessment: this is an important practical result for near-term circuit compilation. The technique isn't asymptotically novel — it's a greedy fitting procedure — but the $O(n^5)$ per-iteration cost and the empirical compression ratios make it immediately useful. The connection between the sum-of-squares picture and the circuit primitive (interleaved Givens + Ising networks) is clean. The convergence slowdown at high accuracy (residual rank saturates) is an honest limitation they flag clearly.

## The algorithm / construction

### Sum-of-squares to circuits

Starting from the sum-of-squares form of $G$, a first-order Trotter approximation gives:

$$e^G \approx e^{-S} \prod_{l=1}^{L} e^{Z_l^2}$$

Each $Z_l$ is a normal one-body operator, so it has an eigenbasis. Diagonalizing $z^{(l)}$ as $z^{(l)} = U_l \,\text{diag}(\lambda^{(l)}) \,U_l^\dagger$ gives:

$$e^{Z_l^2} = U_l \, e^{\sum_{pq} J^{(l)}_{pq} n_p n_q} \, U_l^\dagger$$

where $J^{(l)}_{pq} = \lambda^{(l)}_p \lambda^{(l)}_q$ (outer product of eigenvalues — **not** restricted to rank 1 in the numerical method, unlike analytical decompositions).

The circuit for one Trotter slice is:
1. [[Givens Rotation Slater Determinant Preparation|Givens rotation network]] implementing $\tilde{U}_l = U_l U_{l-1}^\dagger$ — $O(n^2)$ gates, $O(n)$ depth, linear connectivity
2. Ising interaction layer $e^{\sum_{pq} J^{(l)}_{pq} n_p n_q}$ via [[Fermionic Swap Network|swap network]] — $O(n^2)$ gates, $O(n)$ depth, linear connectivity

Total circuit depth per Trotter step: $O(Ln)$ where $L$ is the number of sum-of-squares terms.

### $S_z$ symmetry adaptation

The coefficient supermatrix $\tilde{A}$ decomposes into spin blocks:

$$\tilde{A} = \begin{pmatrix} A & B \\ B^T & C \end{pmatrix}$$

where $A$ encodes $\alpha\alpha$ terms, $C$ encodes $\beta\beta$ terms, and $B$ encodes $\alpha\beta$ cross-terms. Decomposing $A$, $C$, and $B$ separately guarantees all basis rotations stay within a single spin sector, preserving $\hat{S}_z$ symmetry throughout the circuit.

### Analytical decompositions (baselines)

**SVD decomposition:** Reshape $A$ into supermatrix $\tilde{A}$, take its SVD $\tilde{A} = U\sigma V^\dagger$. Each singular triplet gives a pair of one-body operators. Form normal operators via $O \pm iO^\dagger$ combinations. Produces $4 \times \text{rank}(\tilde{A})$ sum-of-squares terms, each with rank-1 $J$ matrices.

**Takagi decomposition:** Uses $\tilde{A} = U\sigma U^T$ (valid since $\tilde{A}$ is complex symmetric). Each column gives $y^{(l)} = \sqrt{\sigma_l}\, u^{(l)}$, made normal via $y^{(l)}_\pm = y^{(l)} \pm i(y^{(l)})^\dagger$. Produces $2 \times \text{rank}(\tilde{A})$ terms — more efficient than SVD by a factor of 2, but still rank-1 constrained.

### The unitary compression algorithm

The greedy procedure:

1. **Optimize:** Given current residual tensor $T$, find a single-particle rotation $U(\kappa) = e^{\sum_{pq} \kappa_{pq} a^\dagger_p a_q}$ that maximizes

   $$O(\kappa) = \sum_{xy} |\tilde{t}_{xx,yy}(\kappa)|^2$$

   i.e., maximizes the total weight in the $n_i n_j$ (diagonal) components of the rotated operator.

2. **Extract:** Store the optimized rotation $\kappa$ and the $n_i n_j$ coefficients $J_{pq}$.

3. **Subtract:** Rotate the extracted diagonal component back to the original basis and subtract from the residual.

4. **Repeat** until the residual norm drops below threshold.

**Gradient computation:** The objective function gradient $\partial O / \partial \kappa_{ab}$ is computed in $O(n^5)$ using the chain rule through the unitary matrix elements $u_{cd} = (e^\kappa)_{cd}$. The key is that the partial derivatives $\partial O / \partial u_{cd}$ (for all $c,d$) cost $O(n^5)$ total, and the Jacobian $\partial u / \partial \kappa_{ab}$ is obtained analytically via the Wilcox identity (no Taylor truncation needed). Total gradient cost: $O(n^5)$ — same as a single basis rotation.

**Iteration cost vs. alternatives:** Least-squares tensor fitting (Cohn, Motta, Parrish 2021) achieves comparable compression but alternates between optimizing $\mu^{(l)}$ and $J^{(l)}$, with unknown convergence properties and numerical difficulties beyond ~6 tensor factors. Unitary compression's greedy structure avoids these issues.

## Key results

**Compression of UCC doubles for HF (12 spin-orbitals) and linear H₄ (16 spin-orbitals):**

| Method | Factors for sub-mHa correlation energy error (HF) | Factors for sub-mHa (H₄) |
|---|---|---|
| SVD | ~60 | ~150 |
| Takagi | ~40 | ~80 |
| Unitary compression | ~10 | ~25 |

The compression ratio is roughly $4\times$ over Takagi and $6\times$ over SVD for these systems.

**ADAPT-VQE for triplet O₂ (STO-3G, 10 orbitals, 16 electrons):**

| Starting state | Overlap with FCI | ADAPT converges to milliHartree accuracy? |
|---|---|---|
| ROHF | $< 10^{-5}$ | No — gets stuck |
| UHF | ~0.35 | Yes (with symmetry breaking) |
| Compressed RO-CCSD (6 factors) | ~CCSD-level | Yes (17 ADAPT iterations for 1 mHa of 2-uCJ energy) |

Unitary compression needs 6 factors to match CCSD correlation energy; Takagi needs 25. This is a $4\times$ circuit depth reduction.

**Convergence behavior:** The residual rank rises quickly to maximum under unitary compression (greedy extraction creates a full-rank low-norm remainder), causing convergence slowdown at high accuracy. The paper suggests either adding a nuclear norm penalty to the objective or switching to Takagi decomposition on the remainder after the greedy phase saturates.

**Two-electron integral compression (Appendix B, naphthalene π-system):** Unitary compression beats Cholesky for max absolute deviation up to ~20 factors, but the resulting truncated Hamiltonian gives poor FCI energies. This is an honest negative result — the greedy objective (max diagonal weight) doesn't align well with preserving spectral properties of the Hamiltonian. The authors correctly flag this as a limitation of the approach for Hamiltonian compression (vs. UCC operator compression where it works well).

## Comparison with prior work

| Method | Rank of $J^{(l)}$ | Terms for UCC | Iteration cost | Notes |
|---|---|---|---|---|
| SVD (Motta et al.) | 1 | $4 \times \text{rank}(\tilde{A})$ | $O(n^3)$ one-time | Analytical, no optimization |
| Takagi (Matsuzawa & Kurashige) | 1 | $2 \times \text{rank}(\tilde{A})$ | $O(n^3)$ one-time | Better than SVD by $2\times$, still rank-1 |
| LS tensor fitting (Cohn et al.) | Full | Comparable to UC | Unknown (alternating opt.) | Numerical difficulties beyond ~6 factors |
| **Unitary compression** (this paper) | **Full** | **$\sim\text{rank}(\tilde{A})/4$** | **$O(n^5)$ per iteration** | Greedy, well-conditioned |
| $k$-uCJ (Matsuzawa & Kurashige) | Full (variational) | $k$ factors (ansatz) | Variational opt. | Related circuit structure, different goal |

The full-rank $J$ matrices are the main advantage over analytical decompositions. Rank-1 $J$ means most of the information capacity per tensor factor is wasted.

## Limits / caveats

- **Convergence slowdown:** The residual rank saturates at maximum, slowing convergence for high-accuracy targets. Not a problem for practical circuits (sub-milliHartree is usually sufficient), but the decomposition is not monotonically efficient.
- **Trotter error not addressed:** The paper decomposes the *generator* but doesn't bound the Trotter error from the [[product formula]] approximation $e^G \approx \prod e^{Z_l^2}$. For near-term applications this is fine (variational parameters absorb it), but for fault-tolerant time evolution you'd need additional analysis.
- **Hamiltonian compression doesn't work well:** As shown in Appendix B, the greedy objective doesn't preserve spectral properties of the Hamiltonian. Don't use this for compressing $H$ directly — use it for compressing $e^{T-T^\dagger}$ generators.
- **$O(n^5)$ per iteration** is cheap but not free. For large basis sets with many iterations, the total classical preprocessing cost could be non-trivial.
- **Greedy only — no global optimality guarantee.** The compression ratio depends on the greedy path. Random vs. Takagi-seeded initialization gives similar results empirically, but there's no proof of optimality.

## Reusable ideas

1. [[Greedy Sum-of-Squares Operator Decomposition]] — The core algorithm: recursively find single-particle rotations maximizing diagonal content, extract, subtract, repeat. Applies to any two-body fermion operator.

2. [[Full-Rank Ising Coupling via Numerical Basis Optimization]] — The observation that relaxing the rank-1 constraint on $J_{pq}$ (inherent in SVD/Takagi) dramatically improves compression. The numerical approach naturally produces full-rank $J$ matrices.

3. [[Compressed UCC as ADAPT-VQE Initial State]] — Using a small number of unitary compression factors to approximate CCSD as a warm start for iterative wavefunction methods.

## References within this paper

Key papers cited:

- **[[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. (2018)]]**, arXiv:1808.02625 — Introduced double factorization / low-rank representations for quantum simulation. The SVD baseline decomposition comes from here.
- **Matsuzawa & Kurashige (2020)**, J. Chem. Theory Comput. 16, 944 — Introduced the Takagi-based decomposition and $k$-uCJ ansatz. Pointed out the rank-1 deficiency problem.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]] — The [[Fermionic Swap Network]] and [[Givens Rotation Slater Determinant Preparation|Givens rotation circuits]] that form the physical circuit primitives.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — The Trotterized [[Unitary Coupled Cluster (UCC) Ansatz|UCC]] circuit strategy that this paper improves upon.
- **Grimsley et al. (2019)**, Nature Commun. 10, 1 — ADAPT-VQE, the iterative circuit construction method used in the application section.
- **Cohn, Motta, Parrish (2021)**, arXiv:2104.08957 — Least-squares tensor fitting alternative; comparable compression but harder to optimize.
- **Lee, Berry, Gidney, Huggins, McClean, Wiebe, Babbush (2020)**, arXiv:2011.03494 — Tensor hypercontraction, another factorization strategy for Hamiltonians that uses a different decomposition path.
- **Yen & Izmaylov (2020)**, arXiv:2007.01234 — Cartan subalgebra approach to measurement, connected via the Lie algebraic perspective on operator decomposition.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — [[Variational Quantum Eigensolver (VQE)|VQE]] hardware demonstration that established UCC as the standard near-term ansatz.
- **Nooijen (2000)**, Phys. Rev. Lett. 84, 2108 — Generalized coupled cluster; the sum-of-squares form in Eq. 3 is a unitary version of this.

## Cross-links

### Paper notes
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Introduced [[Double Factorization of Two-Electron Integrals|double factorization]] and the sum-of-squares circuit structure that this paper's greedy compression directly improves upon; the SVD/Takagi baselines in this paper are the analytical decompositions from Motta et al.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Provides the circuit primitives (Givens networks, swap networks) used to implement the sum-of-squares terms
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — Trotterized UCC compilation that this paper improves
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — VQE + UCC context
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — Same first author (Rubin), VQE measurement reduction context
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Uses QROM/alias sampling for a different factorization approach (qubitization)
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Related initial state overlap concern; compressed UCC addresses the same problem from a different angle
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — measurement reduction for VQE; Rubin is shared coauthor; double factorization appears in both the circuit compilation and measurement contexts
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC is a competing factorization approach; both this paper (sum-of-squares compression) and THC (diagonal-plus-rotation decomposition) target the same two-electron integral problem from different angles; Lee is coauthor on both
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Rubin coauthor; first-quantized alternatives that sidestep the UCC operator compression problem entirely
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — single factorization in the LCU/qubitization setting; the Cholesky structure this paper compresses is the same Cholesky decomposition used in that paper's SF encoding
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — notes difficulty of parallelising factorised derivative measurements; the operator compression framework here is relevant to force operator factorization
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Rubin is first author on both; the Bloch orbital context requires similar operator decompositions for the two-electron integrals
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — shares the Givens rotation / Gaussian unitary structure with matchgate circuits; the double factorization machinery connects to both operator compression and the fermionic measurement framework
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — Rubin is coauthor on both; operator compression informs circuit design for pharmaceutical systems

### Trick cards
- [[Givens Rotation Slater Determinant Preparation]] — Circuit primitive for implementing basis rotations $U_l$
- [[Fermionic Swap Network]] — Circuit primitive for implementing Ising interaction layers
- [[Unitary Coupled Cluster (UCC) Ansatz]] — The operator being compressed
- [[Trotterized Time Evolution for Chemistry]] — The [[product formula]] approximation used
- [[Greedy Sum-of-Squares Operator Decomposition]] — New trick card: the core algorithm
- [[Full-Rank Ising Coupling via Numerical Basis Optimization]] — New trick card: the key improvement over analytical methods
- [[Compressed UCC as ADAPT-VQE Initial State]] — New trick card: the application to warm-starting iterative methods

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
