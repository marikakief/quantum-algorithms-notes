# Quantum Boundary Value Problem via Spectral Method

> **Source:** Childs and Liu, arXiv:1901.00961 (Section 9, Theorem 2)
> **Tags:** #trick #boundary-value-problem #spectral-method #Chebyshev #QLSA #ODE

## What it does
Extends the [[Chebyshev Spectral Discretisation for Quantum ODE Solvers|Chebyshev spectral method]] for quantum ODE solving to handle boundary value problems (BVPs) with $\mathrm{polylog}(1/\varepsilon)$ precision scaling.

## The trick

Given a linear ODE $\dot{x} = A(t)x + f(t)$ on $[0,T]$ with boundary condition $\alpha x(0) + \beta x(T) = \gamma$, the spectral method approximates $x(t)$ as a degree-$n$ Chebyshev series on $[-1,1]$ (mapping $[0,T] \to [-1,1]$).

The linear system is the same as for IVPs except the $l=0$ row: instead of encoding the initial condition $x(t_0) = \gamma$, it encodes the boundary condition

$$\sum_{k=0}^{n} (\alpha_i + (-1)^k \beta_i) c_{i,k} = \gamma_i,$$

since $T_k(1) = 1$ and $T_k(-1) = (-1)^k$.

The key difference from IVPs: **no subinterval decomposition**. The entire $[0,T]$ interval must be covered by a single Chebyshev expansion because the boundary condition couples the endpoints. This means $n$ must be $O(\|A\|T)$ times larger than for IVPs, increasing the condition number and complexity.

The output state encodes $x(t^*)$ for any desired $t^* \in [0,T]$, obtained via the extraction matrix $L_3 = \sum_{i,k} T_k(t^*)|i0\rangle\langle ik|$.

## When to reach for it

- Linear ODE boundary value problems on a quantum computer — the only quantum algorithm specifically designed for BVPs with polylog precision
- Problems where the boundary condition is the natural formulation (e.g., Poisson equation, certain steady-state problems)
- When you need the solution at an interior point $t^*$, not just at the boundaries

## Complexity

$O(\kappa_V s \|A\|^4 T^4 q \cdot \mathrm{poly}(\log(\kappa_V s \|A\| g' T / \varepsilon g)))$. The $\|A\|^4 T^4$ factor (vs $\|A\|T$ for IVPs) comes from the larger Chebyshev order $n = O(\|A\|T \cdot \mathrm{polylog})$ needed without subinterval decomposition.

## Caveat

- The $\|A\|^4 T^4$ scaling makes this expensive for long-time or high-norm problems. For BVPs where $\|A\|T \gg 1$, this can be prohibitive.
- Requires smoothness of the solution, same as the IVP version.
- The $\kappa_V$ dependence is inherited from the IVP analysis.
- Doesn't handle nonlinear BVPs (Carleman linearisation + this method is possible in principle but hasn't been analysed).

## Related notes
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]] — source paper
- [[Chebyshev Spectral Discretisation for Quantum ODE Solvers]] — the IVP version
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]] — alternative spatial discretisation for PDEs (FEM); complementary to spectral methods
