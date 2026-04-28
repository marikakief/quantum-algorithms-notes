# Trotter Error via Nested Commutators for Real-Space Hamiltonians

> **Source:** Childs, Leng, Li, Liu, Zhang, arXiv:2112.05027 (Quantum 2022)
> **Tags:** #trick #trotter #product-formulas #real-space #commutators #kinetic-potential

## What it does

Bounds the $p$-th order product formula error for $H = T + V$ (real-space Schrödinger) in terms of higher spatial derivatives of the potential, exploiting the pseudo-differential structure of the $[T,V]$ commutator to get tight $N$-dependent Trotter step counts.

## The trick

For $H = T + V$ with $T = -\nabla^2 / (2m)$ (kinetic) and $V = V(x)$ (potential, smooth), the $l$-fold nested commutator $[T, [T, \ldots [T, V]\ldots]]$ can be bounded using the fact that $T$ in momentum space is the multiplication operator $k^2/2m$, and commuting $T$ past $V$ generates gradient terms:

$$[T, V] \propto \nabla^2 V + 2(\nabla V)\cdot \nabla$$

and more generally the $l$-fold commutator involves $\nabla^l V$ — the $l$-th spatial derivatives of the potential. In operator norm on a grid of spacing $h = L/N$:

$$\| \underbrace{[T, [T, \ldots [T}_{l}, V]\ldots]] \| = O\!\left(\frac{N^l}{L^l} \cdot \|V^{(l)}\|_\infty\right)$$

The $N^l$ factor comes from the discrete Laplacian eigenvalue $\sim (N/L)^2$.

Plugging into the [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|commutator-based Trotter error bound]] $\|S_p(t) - e^{-iHt}\| \leq C_p \alpha_\text{comm}^{(p+1)} t^{p+1}$ gives the number of Trotter segments needed:

$$r = \tilde{O}\!\left(N t (Nt/\varepsilon)^{1/p}\right)$$

for a $p$-th order formula. Each segment costs $O(p \cdot \eta d \log^2 N)$ gates (QFTs for basis switching). Total:

$$\text{Gates} = \tilde{O}\!\left(N^{1+1/p} t^{1+1/p} \varepsilon^{-1/p}\right)$$

## When to reach for it

- Real-space Schrödinger simulation where you want to choose the product formula order $p$ to minimize gate count for given $N, t, \varepsilon$.
- When comparing product formulas vs. qubitization for the real-space setting (the two methods differ in how they handle the kinetic energy spectrum).
- Any simulation where $T$ is diagonal in a transformed basis (Fourier, cosine, etc.) and $V$ is a smooth multiplication operator — the structure generalizes beyond just $-\nabla^2$.

## Complexity

- Gate complexity at fixed order $p$: $\tilde{O}(N^{1+1/p} t^{1+1/p} \varepsilon^{-1/p})$
- For any $\delta > 0$, choose $p = O(1/\delta)$: gates $= \tilde{O}((Nt)^{1+\delta})$
- Contrast with qubitization/QSP at $O(N^2 t + \log(1/\varepsilon))$: product formulas win for large $N$

## Caveat

- Requires $V$ smooth (all spatial derivatives bounded). Fails for Coulomb $1/r$ without regularization ([[Regularized Coulomb Potential (Delta Cutoff)]]).
- The commutator norm bound has $N^l$-dependence: high $N$ makes low-order Trotter expensive, necessitating high $p$. The near-optimal scaling $(Nt)^{1+o(1)}$ requires astronomically high $p$ in practice — 4th–6th order is the realistic ceiling.
- For multi-particle ($\eta > 1$) pairwise potentials, the commutator analysis extends but the $N$ in the bounds becomes $N^\eta$ (total grid size per particle coordinate).
- The [[Superconvergence via Pseudo-Differential Commutator Cancellation]] trick (An-Fang-Lin 2022 / qHOP) exploits the same pseudo-differential structure to get $N$-independent bounds when working in the interaction picture — a complementary approach.

## Related notes
- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
- [[Near-Optimal Product Formula via Order Scaling]]
- [[Superconvergence via Pseudo-Differential Commutator Cancellation]]
- [[Split-Operator Real-Space Simulation on a Quantum Computer]]
