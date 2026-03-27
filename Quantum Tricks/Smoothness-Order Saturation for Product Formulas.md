
> **Source:** Wiebe, Berry, Høyer, Sanders, arXiv:0812.0562 (J. Phys. A 2010)
> **Tags:** #trick #product-formulas #suzuki #time-dependent #smoothness

## What it does

Establishes that the achievable Suzuki order for time-dependent [[Hamiltonian simulation]] is bounded above by the smoothness class of $H(t)$: if $H$ has only $P$ bounded derivatives, you cannot achieve error scaling better than $O(\Delta\lambda^{P+1})$ with [[Product Formulas]]s, regardless of how deep you run the Suzuki recursion. The recursion saturates at order $P$.

## The trick

The error of the $k$-th order [[Product Formulas]] $U_k$ is $O(\Delta\lambda^{2k+1})$ only if $H(u)$ is $2k$-smooth (i.e., $\|H^{(p)}(u)\|$ is bounded for $p = 0, \ldots, 2k$). If $H$ has only $P < 2k$ bounded derivatives, the Taylor expansion of the ordered exponential truncates at order $P$, and the cancellation mechanism in the Suzuki recursion that eliminates leading-order error terms fails to produce the expected improvement.

**Constructive implication:** For Hamiltonians with finitely many derivatives, set $k = \lfloor P/2 \rfloor$ (you cannot achieve higher effective order) and the formula still gives rigorous bounds — just polynomial, not super-polynomial in $\Delta\lambda/\varepsilon$. This is better than either assuming analyticity (which fails if $H$ has corners or discontinuities in derivatives) or giving up on product formulas entirely.

**Why it matters for $\infty$-smooth $H$:** For Hamiltonians that are $C^\infty$ (e.g., analytic time-dependence from physical control pulses), no smoothness cap applies, and optimizing $k$ over all $\mathbb{N}$ gives near-linear scaling. The saturation principle is what stops this optimization for less-regular $H$.

## When to reach for it

- When checking whether optimized Suzuki order will give you the near-linear-in-$t$ scaling you need: ask first how smooth $H(t)$ is. If it's only $C^2$, don't bother going above order 2.
- When $H(t)$ has known discontinuities (e.g., piecewise-constant control fields, bang-bang schedules): product-formula order is capped at 1 (only zeroth derivative bounded). Use sufficiently small steps and accept $O(\Delta\lambda^2)$ single-step error.
- As a design constraint for optimal-control-theory-informed simulation: smoother pulse shapes → higher achievable Suzuki order → better time scaling.

## Complexity

For $H$ that is exactly $P$-smooth (has $P$ bounded derivatives), optimal order $k = \lfloor P/2 \rfloor$:

$$
N = O\!\left(\left(\frac{\Lambda\Delta\lambda}{\varepsilon}\right)^{1 + 1/P}\right) \quad \text{(polynomial in } \Delta\lambda/\varepsilon\text{)}.
$$

For $C^\infty$ $H$: near-linear $N$ via order optimization.

## Caveat

This is a necessary condition, not just sufficient. The paper does not prove tight lower bounds on what [[Product Formulas]]s can achieve for non-smooth $H$ — just that the Suzuki recursion mechanism breaks. Other approximation methods (Dyson series, Magnus expansion) may perform differently under the same smoothness constraint.

## Related notes
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Suzuki Recursion for Ordered Exponentials]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Λ-Smoothness Parametrization]]
- [[Step-Size-Taylor-Order Balancing for ODE Simulation]]
