
> **Source:** Goings, White, Lee, Tautermann, Degroote, Gidney, Shiozaki, Babbush, Rubin, arXiv:2202.01244
> **Tags:** #trick #quantum-chemistry #tensor-hypercontraction #LCU #qubitization #optimization

## What it does

Produces a tensor hypercontraction (THC) decomposition of the two-electron integral tensor that simultaneously minimizes the factorization error *and* the L1-norm of the central tensor $Z_{PQ}$, which directly controls the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk operator cost ($\lambda = \sum_{PQ} |Z_{PQ}|$ plus one-body terms).

## The trick

Prior THC optimization for qubitized quantum chemistry (as in [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. 2021]]) used random restarts + gradient descent on the least-squares objective. This works but is unreliable for larger systems — the non-convex landscape produces erratically large $|Z_{PQ}|$ values that inflate $\lambda$.

The improved protocol:

**Step 1: Initial guess via CP3 decomposition.** Cholesky-decompose the two-electron integrals:

$$(ij|kl) = \sum_\chi B_{ij,\chi} B_{kl,\chi}$$

Then perform a symmetric canonical polyadic decomposition (CP3) of each Cholesky vector:

$$B_{ij,\chi} = \sum_\tau \beta_{i,\tau} \beta_{j,\tau} \zeta_{\chi,\tau}$$

via alternating least squares (implemented in BTAS). This gives initial THC factors $X_{iP}$ and $Z_{PQ}$ that are already in the right ballpark.

**Step 2: L1-regularized optimization.** Minimize the combined objective:

$$\mathcal{L}(X, Z) = \sum_{ijkl} \left|(ij|kl) - \sum_{PQ} X_{iP} X_{jP} Z_{PQ} X_{kQ} X_{lQ}\right|^2 + C \sum_{PQ} |Z_{PQ}|$$

via L-BFGS-B (from SciPy). The regularization strength $C$ is set so that L2 and L1 terms have equal magnitude at the initial point. Without the L1 term, the optimization can produce decompositions with small L2 error but wildly large $|Z_{PQ}|$ entries — good for chemistry but terrible for quantum resource estimates.

**Validation:** As THC rank $M$ increases, both the L2 norm and $\sum |Z_{PQ}|$ should decrease smoothly. Erratic jumps indicate the optimization got stuck in a bad local minimum.

The empirical THC rank scales as $M \approx 4.7 \times N_{\text{orbitals}}$ for CYP systems, consistent with the $\sim 5N$ rule of thumb from classical quantum chemistry.

## When to reach for it

- Any [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based quantum resource estimation for molecular systems using [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]]
- When brute-force THC optimization (random restarts + gradient descent) produces unstable $\lambda$ values
- Scaling up THC resource estimates to larger molecular systems where optimization reliability matters

## Complexity

CP3 decomposition: $O(r^4 M)$ per alternating least squares iteration, where $r$ is the single-particle basis size. L-BFGS-B optimization: dominated by gradient evaluation, $O(r^4 M)$ per step. Total classical preprocessing cost is polynomial in system size — not a bottleneck relative to DMRG or CCSD(T).

## Caveat

The L1 regularization adds a bias: the resulting THC factorization isn't the *most accurate* possible at a given rank, because you're penalizing large $Z$ entries. The tradeoff is acceptable because the factorization error still needs to be below 1 mHa for the quantum simulation to be useful, and the L1 regularization actually helps reach this target reliably. But if you're using THC for purely classical purposes (e.g., density fitting), you'd skip the L1 term.

The CP3 initial guess requires the BTAS C++ library, adding a build dependency. The optimization is non-convex, so results depend on the initial guess — CP3 just gives a much better starting point than random initialization.

## Related notes
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Single Factorization for Qubitized Chemistry]]
- [[Double Factorization of Two-Electron Integrals]]
