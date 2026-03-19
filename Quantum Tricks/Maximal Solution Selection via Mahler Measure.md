# Maximal Solution Selection via Mahler Measure

> **Source:** Wang, Dong & Lin, arXiv:2110.04993, Quantum **6**, 850 (2022)
> **Tags:** #trick #QSP #phase-factors #Mahler-measure #optimisation

## What it does

Among the combinatorially many global minima of the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] phase-factor optimisation problem, identifies the one with the best convergence properties by selecting roots inside the unit disc.

## The trick

In the [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials|root grouping]] construction, choose the multiset $D$ to contain all roots of $F(z)$ **inside** the unit disc. This yields the complementary polynomial $Q$ with the largest possible leading coefficient magnitude (Lemma 18). The key lower bound uses Jensen's formula / the Mahler measure:

$$q_{d-1}^2 \geq |{\beta}| \cdot \frac{1}{K} \geq 1 - \|f\|_\infty^2$$

where $K = -\prod_{r \in D} r$ and the Mahler measure bound gives $|\beta|/K \geq 1 - \|f\|_\infty^2$.

**Consequence:** the maximal solution's phase factors satisfy $\|\tilde\Phi^* - \tilde\Phi_0\|_2 \leq \frac{\pi}{\sqrt{3}}\|f\|_\infty$, independent of polynomial degree $d$. This is why the fixed initial guess $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$ works — the maximal solution is always nearby, with distance proportional only to $\|f\|_\infty$.

As $\|f\|_\infty \to 0$, the maximal solution converges to $\Phi_0$, which corresponds to $(P, Q) = (iT_d, U_{d-1})$ — pure Chebyshev polynomials.

## When to reach for it

- Proving convergence of optimisation-based [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP/QSVT]] phase-factor algorithms
- Choosing which global minimum to target in phase-factor computation
- Understanding the structure of the QSP energy landscape near $\Phi_0$

## Complexity

The Mahler measure bound itself is free (analytic). The practical value is that it justifies using $\Phi_0$ as a universal initial guess.

## Caveat

The degree-independent distance bound (Theorem 5) holds for $\|f\|_\infty \leq 1/2$, but the local strong convexity guarantee (Theorem 6) requires the more restrictive $\|f\|_\infty = O(d^{-1})$. The gap between these remains an open problem.

## Related notes
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]]
- [[Symmetry Reduction for Phase Factor Uniqueness]]
- [[Recursive Phase Extraction from Polynomial Coefficients]]
