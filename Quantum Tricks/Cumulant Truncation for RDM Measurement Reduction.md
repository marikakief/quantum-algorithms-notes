# Cumulant Truncation for RDM Measurement Reduction

> **Source:** Takeshita, Rubin, Jiang, Lee, Babbush, McClean, arXiv:1902.10679; classical origins in Kutzelnigg & Mukherjee (1997)
> **Tags:** #trick #RDM #measurement #cumulant #approximation #quantum-chemistry

## What it does

Approximates higher-order reduced density matrices (3-RDM, 4-RDM) as antisymmetrized products of lower-order RDMs by zeroing out high-order cumulants. Reduces the number of distinct terms to measure on a quantum device from $\sim N_A^8$ (for the 4-RDM) to $\sim N_A^4$ (using only the 2-RDM).

## The trick

The $l$-particle cumulant ${}^l\Delta$ captures the irreducible $l$-body correlation not expressible as products of lower-order terms. The 4-RDM decomposes as:

$${}^4D = {}^4\Delta + \mathcal{A}\!\left[{}^3\Delta \wedge {}^1D\right] + \mathcal{A}\!\left[{}^2\Delta \wedge {}^2\Delta\right] + \mathcal{A}\!\left[{}^2\Delta \wedge {}^1D \wedge {}^1D\right] + \mathcal{A}\!\left[{}^1D^{\wedge 4}\right]$$

where $\mathcal{A}$ denotes antisymmetrization and $\wedge$ denotes the wedge (antisymmetrized outer) product.

**The approximation:** Set ${}^3\Delta = 0$ and ${}^4\Delta = 0$. Then the 4-RDM is expressed entirely in terms of the 1-RDM and 2-RDM (or equivalently, the 2-cumulant ${}^2\Delta = {}^2D - {}^1D \wedge {}^1D$).

This is the same approximation underlying many classical multi-reference perturbation theories. It's exact for single-determinant states (where all cumulants above order 1 vanish) and degrades gracefully for weakly correlated systems.

## When to reach for it

- You need the 3-RDM or 4-RDM for [[Virtual Quantum Subspace Expansion (VQSE)|VQSE]] or quantum subspace expansion but the $N_A^8$ measurement overhead is infeasible.
- The active-space state has relatively weak higher-order correlations (i.e., the wavefunction is dominated by a few determinants, not a strongly entangled multi-reference state).
- As a first approximation to check whether VQSE gives meaningful improvement, before committing to full 4-RDM measurements.

## Complexity

- **Without truncation:** $\sim N_A^8$ distinct 4-RDM elements to measure.
- **With truncation (${}^3\Delta = {}^4\Delta = 0$):** $\sim N_A^4$ (only 2-RDM needed, plus $O(N_A^8)$ classical postprocessing to reconstruct the approximate 4-RDM).
- **Systematically improvable:** Include ${}^3\Delta$ (measure 3-RDM, $\sim N_A^6$ terms) for better accuracy, then ${}^4\Delta$ for exact results.

## Caveat

- The approximation error from dropping high-order cumulants is hard to bound a priori. For strongly correlated states (large active spaces with many near-degenerate configurations), the 3- and 4-body cumulants can be significant.
- Cumulant truncation can break $n$-representability of the reconstructed 4-RDM — the approximate 4-RDM may not correspond to any physical state. This can cause unphysical results in the downstream VQSE calculation.
- Not useful if you already need the 4-RDM for other purposes (e.g., full QSE within the active space).

## Related notes
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]]
- [[Virtual Quantum Subspace Expansion (VQSE)]] — primary use case for this trick
- [[Variance Reduction via N-Representability Constraints (LP Method)]] — complementary measurement reduction approach
- [[2-RDM Physicality Restoration by PSD Projection]] — can restore physicality after cumulant truncation breaks it
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
