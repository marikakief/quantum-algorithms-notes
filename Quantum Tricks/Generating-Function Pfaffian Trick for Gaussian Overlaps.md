# Generating-Function Pfaffian Trick for Gaussian Overlaps

> **Source:** Wan, Huggins, Lee, Babbush, arXiv:2207.13723
> **Tags:** #trick #Pfaffian #generating-function #Gaussian-state #classical-shadows #matchgate #Clifford-algebra

## What it does

Computes the projection of a Gaussian state onto each even-Majorana sector $\Gamma_{2\ell}$ in $O(n^3)$ total time, by encoding all projections as coefficients of a single Pfaffian-valued polynomial and using a derivative recursion to extract them.

## The trick

Given two Gaussian states $\varrho_1$, $\varrho_2$ with covariance matrices $C_{\varrho_1}$, $C_{\varrho_2}$, introduce a formal variable $z$ that tags the grade of $\varrho_2$:

$$p_{\varrho_1, \varrho_2}(z) = \sum_{\ell=0}^{n} \operatorname{tr}(\varrho_1 \, P_{2\ell}(\varrho_2)) \, z^\ell$$

where $P_{2\ell}$ projects onto $\Gamma_{2\ell}$. The key identity (Theorem 2): for invertible $C_{\varrho_1}$,

$$p_{\varrho_1, \varrho_2}(z) = \frac{1}{2^n} \operatorname{pf}(C_{\varrho_1}) \, \operatorname{pf}(-C_{\varrho_1}^{-1} + z \, C_{\varrho_2})$$

The proof works in the Clifford algebra. Write each Gaussian state as $\varrho = \prod_j \frac{1}{2}(I - i\lambda_j \tilde{\gamma}_{2j-1}\tilde{\gamma}_{2j})$. Expanding and tracking grades, the $z^\ell$ coefficient isolates products of exactly $\ell$ of the bivectors from $\varrho_2$. The trace becomes a scalar (grade-0) part in the Clifford algebra. Using the wedge product structure and the multinomial theorem to reassemble the sum, the whole expression collapses to a Pfaffian via Fact 3 (the standard identity relating the $n$-th exterior power of a bivector to its Pfaffian).

**Extracting coefficients in $O(n^3)$:** The polynomial $p(z) = \operatorname{pf}(A(z))$ with $A(z) = B + zC$ linear in $z$ has degree $\leq r$ (where $2r = \operatorname{rank}(C_{\varrho_1})$). The $\ell$-th coefficient is $\partial^\ell p / \partial z^\ell |_{z=0} / \ell!$. From $\operatorname{pf}(A)^2 = \det(A)$ and Jacobi's formula:

$$\partial^1 \operatorname{pf}(A) = \frac{1}{2} \operatorname{pf}(A) \operatorname{tr}(A^{-1} C)$$

Higher derivatives follow from the product rule. Precompute the eigenvalues $\lambda_1, \ldots, \lambda_{2r}$ of $B^{-1}C$ in $O(r^3)$ time, then $\operatorname{tr}((B^{-1}C)^{j+1}) = \sum_i \lambda_i^{j+1}$ gives all needed derivatives in $O(r^2)$ total. The recursion $\partial^\ell f = \sum_{j=0}^{\ell-1} \binom{\ell-1}{j} (\partial^{\ell-1-j} f)(\partial^j g)$ then fills in all coefficients iteratively.

## When to reach for it

- Computing matchgate shadow estimates of Gaussian state fidelities $\operatorname{tr}(\varrho \rho)$: each shadow sample requires evaluating $p_{\varrho, U_Q^\dagger |b\rangle\langle b| U_Q}(z)$ and combining the coefficients with the inverse channel weights.
- More generally, whenever you need the grade decomposition $\operatorname{tr}(A \, P_{2\ell}(B))$ for Gaussian operators $A$, $B$.
- The same polynomial interpolation / derivative approach works for Theorem 3 (Slater determinant overlaps), where a similar Pfaffian polynomial appears with a modified basis change.

## Complexity

- **Total for all $n+1$ coefficients:** $O(r^3)$, dominated by the eigenvalue computation. This is better than the $O(r^4)$ from naive polynomial interpolation.
- **Per shadow sample for fidelity estimation:** $O(n^3)$ (one eigendecomposition + one linear recurrence).

## Caveat

Requires $C_{\varrho_1}$ to have rank $2r > 0$. For rank-deficient covariance matrices (mixed Gaussian states with some $\lambda_j = 0$), a block decomposition is needed: rotate $C_{\varrho_1}$ to isolate its nonzero block, restrict the computation to that block, and account for the zero eigenvalues separately. The general formula (Eq. 55 in the paper) handles this but adds a basis change step.

The trick is specific to free-fermion structure. It doesn't generalize to interacting systems (where Wick's theorem fails).

## Related notes

- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]]
- [[Pfaffian Shadow Estimation for Fermionic Observables]] — the local-observable special case
- [[Grassmann Integral Framework for Free-Fermion Traces]] — the general framework that extends beyond Gaussian-Gaussian traces
- [[Shadow Tomography for QMC Overlap Estimation]] — the Clifford shadow approach that matchgate shadows replace
- [[Givens Rotation Slater Determinant Preparation]] — Gaussian unitaries used in the shadow protocol
