# Löwdin Thresholding for Noisy Generalized Eigenvalue Problems

> **Source:** Epperly, Lin, and Nakatsukasa, arXiv:2110.07492 (SIAM J. Matrix Anal. Appl. 43(3), 2022)
> **Tags:** #trick #generalized-eigenvalue #thresholding #noise #perturbation-theory

## What it does

Makes noisy generalized eigenvalue problems $Hc = ESc$ solvable by discarding the small-eigenvalue subspace of the overlap matrix $S$, even when $S$ is nearly singular and the noise is large relative to machine precision.

## The trick

Given noisy matrices $\tilde{H} = H + \Delta H$ and $\tilde{S} = S + \Delta S$ with noise level $\eta = \sqrt{\|\Delta H\|^2 + \|\Delta S\|^2}$:

1. Diagonalise $\tilde{S} = VDV^*$
2. Set threshold $\varepsilon$; let $I = \{i : D_{ii} > \varepsilon\}$
3. Project: $\tilde{A} = V_I^* \tilde{H} V_I$, $\tilde{B} = V_I^* \tilde{S} V_I$
4. Solve the reduced (now well-conditioned) eigenvalue problem

This is Löwdin canonical orthogonalisation — a standard technique in quantum chemistry — but the paper provides the first rigorous analysis of why it works for QSD under noise.

**Key insight:** Classical worst-case bounds give error $\sim \eta/\varepsilon$ (divergent as $\varepsilon \to 0$). But QSD matrices satisfy a **geometric-mean condition**:

$$|v_i^* H v_j| \leq \mu[\min(\lambda_i, \lambda_j)]^{1-\alpha}[\max(\lambda_i, \lambda_j)]^{\alpha}$$

Under this condition, the optimal threshold is $\varepsilon = \Theta(\eta^{1/(1+\alpha)})$ and the eigenangle error scales as $O(d_0^{-1}\eta^{1/(1+\alpha)})$ — sublinear in $\eta$.

For $\alpha \approx 1/4$ (empirically typical), this gives error $\sim \eta^{4/5}$ with threshold $\varepsilon \sim \eta^{4/5}$.

## When to reach for it

- Quantum subspace diagonalisation (QSD) with Monte Carlo noise from Hadamard-test measurements.
- Any setting where you have a noisy generalised eigenvalue problem $(H + \Delta H, S + \Delta S)$ with $S$ nearly singular but structured.
- Quantum chemistry codes that solve Roothaan-type equations with near-linearly-dependent basis sets.

## Complexity

The classical cost is the eigendecomposition of $\tilde{S}$ ($O(n^3)$ for $n \times n$ matrices) plus the reduced eigenvalue problem ($O(q^3)$ where $q = |I|$ is the number of retained eigenvalues). Both are negligible compared to the quantum measurement cost.

## Caveat

- The geometric-mean condition parameter $\alpha$ must be estimated empirically — there's no a priori guarantee it takes a favorable value.
- The $n^3$ factor in the theoretical bound (Theorem 2.7) is likely pessimistic.
- If the ground eigenvector has large norm in the $S$-basis ($\|c_0\|$ large), thresholding can introduce significant error (Theorem 4.2).
- Does **not** apply to arbitrary ill-conditioned generalized eigenvalue problems; the structure of QSD-generated matrices is essential.

## Related notes
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]]
- [[Trigonometric Minimax Polynomial for Krylov Concentration]]
- [[Approximate CDF from Hadamard Tests]] — alternative classical post-processing for the same quantum data
- [[Chebyshev Polynomial Spectral Projector]]
