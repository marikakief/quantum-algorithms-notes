
> **Source:** Cho, Berry, Hsieh, arXiv:2210.11281; builds on Yoshida (1990)
> **Tags:** #trick #product-formulas #error-analysis #symmetry

## What it does

Proves that half the error terms in a symmetric [[Product Formulas]] automatically vanish without any correction, because the BCH expansion of the symmetrised error contains only odd-order terms.

## The trick

For a symmetric [[Product Formulas]] $S_{2k}$ satisfying time-reversibility $S_{2k}(\lambda) S_{2k}(-\lambda) = \mathbb{1}$, and exact evolution $V(\lambda)$ (also time-reversible), the product $(V^\dagger S_{2k})^{-1}(S_{2k} V^\dagger)^{-1} = S_{2k}^\dagger V V S_{2k}^\dagger$ is unitary and time-reversible. By the Yoshida lemma, its logarithm contains only odd powers of $\lambda$.

Since the product equals $\mathbb{1}$ up to order $2k$ (the formula is accurate to that order), the first non-zero term is at order $2k+1$. The next non-zero terms are at orders $2k+3, 2k+5, \ldots$, with all even orders ($2k+2, 2k+4, \ldots$) vanishing identically.

This means the correction $V^\dagger D + D V^\dagger$ (which equals this product minus identity, up to order $4k+1$) needs to be computed only at the $k+1$ odd orders $\{2k+1, 2k+3, \ldots, 4k+1\}$ — half the work.

## When to reach for it

- Designing correction schemes (randomized or deterministic) for symmetric [[Product Formulas]]s: you only need to correct odd-order terms.
- Analysing how many independent correction parameters are needed to boost accuracy by a given number of orders.
- Any setting where Suzuki-type symmetric formulas are used and you want to understand which error terms are structurally zero.

## Complexity

No computational cost — this is a structural observation that reduces the number of correction terms by a factor of 2.

## Caveat

Only applies to symmetric (palindromic) [[Product Formulas]]s. Asymmetric formulas (e.g., first-order Lie-Trotter) have both even and odd error terms. Also, the order-$4k+2$ term can be non-zero because it arises from squaring the order-$2k+1$ term in the exponential.

## Related notes
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]]
- [[Randomized Correction to Double Product-Formula Order]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Symplectic Corrector Injection for Product Formulas]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Bernoulli Polynomial Kernel Expansion for Product Formulas]]
