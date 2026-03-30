# Chebyshev Spectral Discretisation for Quantum ODE Solvers

> **Source:** Childs and Liu, arXiv:1901.00961
> **Tags:** #trick #spectral-method #Chebyshev #ODE #QLSA #precision

## What it does
Approximates the solution of a linear ODE globally using a truncated Chebyshev series, producing a linear system for the coefficients that can be solved by a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] with $\mathrm{polylog}(1/\varepsilon)$ error scaling — even for time-dependent coefficients.

## The trick

Given $\dot{x} = A(t)x + f(t)$ on $[0, T]$, divide into $m = O(\|A\|T)$ subintervals, map each to $[-1, 1]$, and approximate the solution on each subinterval as

$$x_i(t) = \sum_{k=0}^{n} c_{i,k} T_k(t),$$

where $T_k$ are Chebyshev polynomials of the first kind. Enforce the ODE at the $n+1$ Chebyshev-Gauss-Lobatto nodes $t_l = \cos(l\pi/n)$:

$$\sum_k [D_n]_{kj} c_{i,j} \cdot T_k(t_l) = \sum_j A_{ij}(t_l) \sum_k c_{j,k} T_k(t_l) + f_i(t_l),$$

where $D_n$ is the Chebyshev differentiation matrix (upper triangular, entries $[D_n]_{kj} = 2j/\sigma_k$ for odd $k+j$, $j > k$).

This gives a linear system $L|X\rangle = |B\rangle$ where $L$ is $(m+p+1)d(n+1)$-dimensional, with the QLSA solving for the Chebyshev coefficients. The solution at any desired time is then a linear combination of these coefficients.

**Why it works:** For $C^\infty$ solutions, the Chebyshev approximation error is $O(g' \cdot (e/2n)^n)$ — super-exponential in $n$. This means $n = O(\log(g'/\varepsilon)/\log\log(g'/\varepsilon))$ suffices, which is polylogarithmic in $1/\varepsilon$. Compare finite differences: $O(h^k)$ error requires $n = O((1/\varepsilon)^{1/k})$ points — always polynomial.

## When to reach for it

- Solving linear ODEs with time-dependent, smooth coefficients on a quantum computer — this was the first method achieving $\mathrm{polylog}(1/\varepsilon)$ for such problems
- When the solution is smooth ($C^\infty$ or high regularity) — spectral convergence gives the biggest win here
- Boundary value problems — the global approximation naturally accommodates boundary conditions at both endpoints
- When you want a unified framework for IVPs and BVPs

## Complexity

For IVPs: $O(\kappa_V s \|A\| T q \cdot \mathrm{poly}(\log(\kappa_V s \|A\| g' T / \varepsilon g)))$ queries, where $g' = \max_{t,n}\|x^{(n+1)}(t)\|$ measures solution smoothness.

For BVPs: $O(\kappa_V s \|A\|^4 T^4 q \cdot \mathrm{poly}(\log(\cdots)))$ — worse because no subinterval decomposition.

## Caveat

- Requires smooth solutions for the exponential convergence. Solutions with finite regularity $C^{r+1}$ degrade to $O(1/n^{r-2})$ algebraic convergence.
- Depends on $\kappa_V$ (condition number of the diagonalising matrix), which can be exponentially large for non-normal $A(t)$. Later work ([[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi 2023]]) eliminates this.
- The smoothness parameter $g'$ can be hard to bound for general time-dependent problems.
- The [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry-Costa Dyson series approach]] (2022) later achieved similar scaling without the spectral method machinery, making this somewhat superseded for IVPs. Still the natural choice for BVPs.

## Related notes
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]] — source paper
- [[Quantum Boundary Value Problem via Spectral Method]] — BVP extension
- [[History-State Linear System Encoding for ODE Trajectories]] — the general encoding framework
- [[Forward Euler to Linear System Reduction]] — the finite-difference alternative (worse precision scaling)
- [[Taylor-Series-to-Linear-System Encoding]] — another alternative (polylog precision, but time-independent only until Berry-Costa)
- [[Chebyshev Polynomial Spectral Projector]] — related Chebyshev technique for different problems
