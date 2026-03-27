
> **Source:** An, Childs, Lin, arXiv:2312.03916 (Theorem 6)
> **Tags:** #trick #LCHS #non-unitary #hamiltonian-simulation #identity

## What it does

Expresses a general non-unitary propagator $\mathcal{T} e^{-\int_0^t A(s)\, ds}$ (with dissipative $A$) as a continuous [[Linear Combination of Unitaries (LCU)|LCU]] of unitary [[Hamiltonian simulation]] operators, with a **freely choosable** kernel function rather than the fixed Cauchy kernel of the original [[LCHS Kernel for Non-Unitary Dynamics|LCHS]].

## The trick

Write $A(t) = L(t) + iH(t)$ with $L \succeq 0$. Then for any kernel $f(z)$ that is analytic on the lower half-plane, has sufficient decay, and satisfies $\int_\mathbb{R} f(k)/(1-ik)\, dk = 1$:

$$\mathcal{T} e^{-\int_0^t A(s)\, ds} = \int_\mathbb{R} \frac{f(k)}{1-ik}\, \mathcal{T} e^{-i\int_0^t [kL(s) + H(s)]\, ds}\, dk.$$

The original LCHS identity used $f(k) = 1/(\pi(1+ik))$, giving the Cauchy kernel $1/(\pi(1+k^2))$ with $O(1/k^2)$ polynomial decay. By choosing the [[Near-Optimal Hardy-Space Kernel for LCHS|Hardy-space kernel]] $f_\beta(k) = (1/C_\beta) e^{(1+ik)^\beta}$, the decay becomes $e^{-c|k|^\beta}$ — sub-exponential. This exponentially reduces the number of quadrature points needed to discretise the integral.

The freedom to choose $f$ is what makes the improved LCHS possible. The only constraints on $f$ are analyticity, decay, and normalisation — a large function class.

## When to reach for it

Whenever you need to implement a non-unitary operator $e^{-tA}$ with $A$ dissipative (positive Hermitian part) and you have access to [[Hamiltonian simulation]] of $kL + H$. This is the foundation for:
- Quantum ODE solvers (this paper)
- [[Laplace-Transform LCU Lifting for Eigenvalue Transforms|Lap-LCHS]] for general eigenvalue transforms
- Gibbs state preparation
- Any application where the LCHS idea applies but the original Cauchy kernel is too slow

## Complexity

The number of quadrature points scales as $M = O(T \max_t \|L(t)\| \cdot (\log 1/\varepsilon)^{1+1/\beta})$ for the time-dependent case, or $O(\alpha_A T \cdot (\log 1/\varepsilon)^{1/\beta})$ for time-independent.

## Caveat

Requires $L(t) \succeq 0$. The proof depends on contour integration in the lower half-plane, which needs the unitary integrand to decay as $\mathrm{Im}(k) \to -\infty$. If $L$ has negative eigenvalues, the identity doesn't hold.

## Related notes

- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]] — the original (Cauchy kernel) version
- [[Near-Optimal Hardy-Space Kernel for LCHS]] — the kernel that makes this identity useful
- [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]] — why you can't push to full exponential decay
- [[Adaptive Kernel Parameter for LCHS]] — optimising the free parameter $\beta$
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] — extends this to general eigenvalue transforms
