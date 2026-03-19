# Laurent Polynomial Root Grouping for QSP Complementary Polynomials

> **Source:** Wang, Dong & Lin, arXiv:2110.04993, Quantum **6**, 850 (2022)
> **Tags:** #trick #QSP #phase-factors #root-finding #Laurent-polynomial #complementary-polynomial

## What it does

Constructs **all** admissible complementary polynomials $(P_{\mathrm{Im}}, Q)$ for a given target polynomial $f$ in [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]], by grouping roots of a Laurent polynomial.

## The trick

Given target $f(x)$ with $\|f\|_\infty < 1$, form

$$F(z) = 1 - \left[f\!\left(\tfrac{z+z^{-1}}{2}\right)\right]^2$$

which has $4d$ roots. Select a multiset $D$ of $2d$ roots such that:
1. $D \cup D^{-1}$ contains all roots (with multiplicity)
2. $D$ is closed under complex conjugation
3. $D$ is closed under negation (for the parity constraint)

Then set $e(z) = z^{-d}\prod_{r \in D}(z - r)$ and construct:

$$P_{\mathrm{Im}}(x) = \sqrt{\alpha}\,\frac{e(x+i\sqrt{1-x^2}) + e(x-i\sqrt{1-x^2})}{2}$$

$$Q(x) = \sqrt{\alpha}\,\frac{e(x+i\sqrt{1-x^2}) - e(x-i\sqrt{1-x^2})}{2i\sqrt{1-x^2}}$$

where $\alpha > 0$ is determined by $F(z) = \alpha\,e(z)\,e(z^{-1})$.

Different root groupings $\to$ different admissible pairs $\to$ different global minima of the QSP optimisation problem. This gives a complete parameterisation.

## When to reach for it

- Analysing the structure of [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP/QSVT]] phase-factor solutions
- Proving properties of specific phase-factor choices (e.g., the [[Maximal Solution Selection via Mahler Measure|maximal solution]])
- Understanding why the QSP optimisation landscape has many global minima

## Complexity

Requires finding $4d$ roots of a degree-$4d$ Laurent polynomial, then partitioning them. Root-finding is numerically unstable for large $d$ — that's the whole reason optimisation-based methods exist as an alternative.

## Caveat

This construction is analytically useful but **not** a practical algorithm for large $d$, because the root-finding step requires $O(d\log(d/\epsilon))$-bit extended precision arithmetic. The [[Partial-Quadrature Completion via Root Grouping]] trick from Low-Yoder-Chuang (2016) is related but operates in the monomial/SU(2) framework rather than Laurent polynomials.

## Related notes
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[Partial-Quadrature Completion via Root Grouping]]
- [[Recursive Phase Extraction from Polynomial Coefficients]]
- [[Maximal Solution Selection via Mahler Measure]]
- [[Symmetry Reduction for Phase Factor Uniqueness]]
