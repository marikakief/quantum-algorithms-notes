# Discontinuity Excision for Hamiltonian Simulation

> **Source:** Wiebe, Berry, Høyer & Sanders, arXiv:1011.3489 (2011)
> **Tags:** #trick #hamiltonian-simulation #time-dependent #discontinuities

## What it does

Handles Hamiltonians with isolated discontinuities (finitely many jump times) by removing $\delta$-neighborhoods around each discontinuity, bounding the omitted evolution by a simple norm estimate, and simulating the smooth remainder normally.

## The trick

Suppose $H(t)$ is $\Lambda\text{-}2k$-smooth on $[t_0, t_0+\Delta t]$ except at $L$ isolated times $t_1, \ldots, t_L$.

1. **Choose excision width.** Let $\delta$ be small enough that removing $\delta$-windows costs at most $\varepsilon/2$ in error. Since $\|e^{iH_{\max}\delta} - I\| \leq e^{H_{\max}\delta} - 1 \approx H_{\max}\delta$ for small $\delta$, set $\delta = \varepsilon/(2LH_{\max})$.

2. **Simulate the smooth pieces.** Split $[t_0, t_0+\Delta t]$ into $L+1$ smooth intervals (excising $[t_\ell - \delta, t_\ell + \delta]$ around each discontinuity). Simulate each piece with the order-$k$ Suzuki integrator to error $\varepsilon/(2(L+1))$.

3. **Count.** The total query cost acquires an additive $(L+1)$ factor plus the smooth simulation cost:

$$\text{QueryCost} \leq 12CMd^2 5^{k-1}\left[(L+1) + 24kd^2\Lambda\Delta t\left(\frac{5}{3}\right)^k\left(\frac{6d^2\Lambda\Delta t}{\varepsilon/3}\right)^{1/(2k)}\right].$$

The $L$ term is additive — each discontinuity costs $O(1)$ extra simulation steps, not a multiplicative overhead.

**Key requirement:** the mesh size $\Delta t/2^{n_t}$ must be smaller than the gap between consecutive discontinuities ($\min_\ell(t_{\ell+1} - t_\ell)$), so the time oracle can resolve all smooth pieces without straddling a jump.

## When to reach for it

- Piecewise-smooth $H(t)$ with known breakpoints (pulsed control fields, sudden quenches, periodic kicks)
- Any setting where $|\partial_t H|$ blows up at isolated times but is bounded away from them
- When the [[Adaptive Timestep via Local Smoothness Control]] approach fails (that requires $|\partial_t\Upsilon| \leq K^2\Upsilon^2$, which a genuine step discontinuity violates)

## Complexity

Adding $O(L)$ to the query cost. As long as $L$ is polynomial, the total remains efficient.

## Caveat

Requires knowing the discontinuity times in advance (or at least their locations within the time mesh). If discontinuities are dense or unknown, this approach doesn't apply. Also, the $\delta$-excision argument requires $H$ to be uniformly bounded near the jumps ($H_{\max} < \infty$).

## Related notes
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Adaptive Timestep via Local Smoothness Control]]
- [[Λ-Smoothness Parametrization]]
- [[Smoothness-Order Saturation for Product Formulas]]
