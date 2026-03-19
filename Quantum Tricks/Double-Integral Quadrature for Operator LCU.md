# Double-Integral Quadrature for Operator LCU

> **Source:** An, Childs, Lin, arXiv:2411.04010
> **Tags:** #trick #quadrature #LCU #discretisation

## What it does

Converts a continuous two-parameter [[Linear Combination of Unitaries (LCU)|LCU]] — an integral $\int\int w(k,t)\, U(k,t)\, dk\, dt$ — into a finite weighted sum suitable for quantum implementation, with controlled truncation and discretisation error.

## The trick

Given $h(A) = \int_0^\infty \int_\mathbb{R} w(k,t)\, e^{-it(kL+H)}\, dk\, dt$:

1. **Truncate:** Restrict $k \in [-K, K]$, $t \in [0, T]$. Choose $K$ large enough that $\|f\|_{L^1(\mathbb{R} \setminus [-K,K])} \leq \varepsilon_1$ and $T$ large enough that $\|g\|_{L^1((T,\infty))} \leq \varepsilon_2$.

2. **Discretise:** Apply Riemann sums (or higher-order quadrature) with spacings $h_k$, $h_t$, giving $M_k \times M_t$ grid points. Quadrature error bounded by $O(h_k \|f'\|_{L^1} + h_t \|g'\|_{L^1})$ (first order) or better with higher-order rules.

3. **Encode weights:** Prepare a quantum state $\sum_{j,l} \sqrt{|w_{j,l}|}\, |j,l\rangle$ encoding the quadrature weights, with phase information handled by controlled rotations. This is the PREPARE oracle for the LCU.

4. **SELECT:** Conditionally apply $e^{-it_l(k_j L + H)}$ (Hamiltonian simulation) controlled on $|j,l\rangle$.

The total number of unitaries in the sum is $M_k \times M_t$, but each is just a Hamiltonian simulation call. The PREPARE oracle costs $O(\text{polylog}(M_k M_t))$ assuming efficient state preparation.

## When to reach for it

Any time you have a continuous operator integral that decomposes into a 2D family of unitaries. Beyond [[Laplace-Transform LCU Lifting for Eigenvalue Transforms|Lap-LCHS]], this pattern appears in contour-integral methods for matrix functions and certain quantum ODE solvers.

## Complexity

Grid size: $M_k = O(K/h_k)$, $M_t = O(T/h_t)$. For first-order quadrature, $h_k, h_t = O(\varepsilon)$, giving $M_k M_t = O(KT/\varepsilon^2)$ points. Higher-order quadrature reduces this. Each grid point costs one Hamiltonian simulation of time $\leq KT$.

## Caveat

The number of quadrature points can be large if the kernel or $g$ has fine structure (sharp peaks, oscillations). Higher-order quadrature helps only if the integrands are sufficiently smooth.

## Related notes

- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]]
- [[Laplace-Transform LCU Lifting for Eigenvalue Transforms]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Linear Combination of Unitaries (LCU)]]
