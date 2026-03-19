# Symmetry Reduction for Phase Factor Uniqueness

> **Source:** Wang, Dong & Lin, arXiv:2110.04993, Quantum **6**, 850 (2022)
> **Tags:** #trick #QSP #phase-factors #symmetry #optimisation #Hessian

## What it does

Converts the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] phase-factor problem from an over-determined system (singular Hessian, infinite global minima) into a well-conditioned one (positive-definite Hessian, finitely many global minima) by imposing symmetry on the phase factors.

## The trick

Restrict phase factors to be **symmetric**: $\Phi = (\phi_0, \phi_1, \ldots, \phi_1, \phi_0)$. This halves the free variables from $d+1$ to $e_d = \lceil(d+1)/2\rceil$, which matches the $e_d$ constraints from the target polynomial's parity-restricted degrees of freedom.

Without symmetry:
- $d+1$ variables vs $e_d$ constraints $\Rightarrow$ under-determined
- Hessian is **always singular** at global minima (kernel dimension $\geq d - e_d$)
- Infinite continuum of global minima

With symmetry:
- $e_d$ variables vs $e_d$ constraints $\Rightarrow$ square system
- Hessian at $\Phi_0$ is $4I$ (odd $d$) or $\mathrm{diag}(4,\ldots,4,2)$ (even $d$) — full rank
- **Unique** symmetric phase factors for each admissible pair $(P,Q)$ (Theorem 1)
- Finitely many global minima (one per admissible pair)

The symmetry constraint is compatible with the [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP existence theorem]] — any achievable polynomial can be realised with symmetric phases.

## When to reach for it

- Designing optimisation-based QSP phase-factor algorithms (always impose symmetry)
- Analysing convergence of gradient methods for QSP (need non-singular Hessian)
- Reducing the search space for phase-factor enumeration

## Complexity

No additional cost — symmetry just constrains the search space. Gradient computation is slightly cheaper because there are half as many free variables.

## Caveat

Symmetry makes the problem tractable for analysis, but the strong convexity result still requires $\|f\|_\infty = O(d^{-1})$. In practice, non-symmetric optimisation also converges (just can't be proven to).

## Related notes
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — first paper to exploit symmetric phases for optimisation
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[Maximal Solution Selection via Mahler Measure]]
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — uses symmetric phase factors via QSPPACK
- [[Chebyshev-Node Loss for QSP Phase Optimisation]]
- [[Perturbative Initial Guess for QSP Phases]]
