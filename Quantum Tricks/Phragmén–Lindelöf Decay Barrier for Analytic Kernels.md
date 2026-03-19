# Phragmén–Lindelöf Decay Barrier for Analytic Kernels

> **Source:** An, Childs, Lin, arXiv:2312.03916 (Proposition 8)
> **Tags:** #trick #impossibility #Hardy-space #LCHS #complex-analysis

## What it does

Proves that any kernel function $f(z)$ used in the [[Generalised LCHS Identity|generalised LCHS identity]] — satisfying analyticity on the lower half-plane, boundedness, and the normalisation condition — **cannot** have true exponential decay $|f(k)| \leq C' e^{-c|k|}$ on the real axis. Such a function would be identically zero.

## The trick

Suppose $f(z)$ is analytic on $\{\mathrm{Im}(z) < 0\}$, continuous and bounded on $\{\mathrm{Im}(z) \leq 0\}$, and satisfies $|f(k)| \leq C' e^{-c|k|}$ for $k \in \mathbb{R}$. Define $g(z) = f(z) \cdot e^{-icz}$. Then $g$ is bounded by 1 on the real axis (the exponential growth from $e^{-icz}$ in the lower half-plane is exactly cancelled by the assumed decay on $\mathbb{R}$), and $|g(z)|$ has at most exponential growth of arbitrarily small rate in the lower half-plane. The Phragmén–Lindelöf principle (a generalisation of the maximum modulus principle for unbounded domains) then forces $|g(z)| \leq 1$ everywhere in the closed lower half-plane. Repeating the argument with $e^{+icz}$ gives $|f(z)| \leq e^{-c|\mathrm{Re}(z)|}$ on $\{\mathrm{Im}(z) \leq 0\}$, which means $f \to 0$ as $|\mathrm{Re}(z)| \to \infty$ uniformly in $\mathrm{Im}(z)$. Combined with bounded total variation, this forces $f \equiv 0$.

## When to reach for it

When designing LCHS kernels and wondering whether you can push the decay rate to full exponential. You can't — this sets the fundamental barrier. The [[Near-Optimal Hardy-Space Kernel for LCHS|sub-exponential family]] $e^{-c|k|^\beta}$ with $\beta < 1$ is the best you can do.

## Complexity

N/A — this is a negative result.

## Caveat

The barrier is specific to kernels that must be analytic on the lower half-plane (a requirement from the contour-integral proof of the [[Generalised LCHS Identity|LCHS identity]]). If a different proof strategy removed this analyticity requirement, the decay barrier might not apply. But no such strategy is known.

## Related notes

- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Generalised LCHS Identity]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
