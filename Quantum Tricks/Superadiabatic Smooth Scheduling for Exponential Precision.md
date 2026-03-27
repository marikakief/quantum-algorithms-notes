
> **Source:** Dong An and Lin Lin, arXiv:1909.05500; uses Nenciu (1993) superadiabatic theory
> **Tags:** #trick #adiabatic #superadiabatic #precision #scheduling

## What it does

Achieves **polylogarithmic dependence on precision** ($1/\varepsilon$) in adiabatic quantum computing by using a scheduling function whose derivatives of all orders vanish at the endpoints.

## The trick

Use a smooth schedule like:

$$f(s) = \frac{1}{c_e} \int_0^s \exp\!\left(-\frac{1}{s'(1-s')}\right) ds',$$

where $c_e$ normalises $f(1) = 1$. This is a smooth bump-function antiderivative: $f^{(k)}(0) = f^{(k)}(1) = 0$ for all $k \geq 1$.

Because all derivatives of the Hamiltonian $H(f(s))$ with respect to $s$ vanish at the endpoints, the Nenciu (1993) superadiabatic perturbation theory applies. The key mechanism: at intermediate orders of the adiabatic expansion, the boundary terms that normally limit accuracy all vanish. The final-time error is bounded by the **optimally truncated** asymptotic series, which decays exponentially in a power of $T$:

$$\text{error} \leq C \exp\!\left[-C'\left(g(T)\right)^{-1/4}\right],$$

where $g(T)$ depends on the gap profile and $T$.

This converts polynomial precision scaling ($T = O(1/\varepsilon)$) into polylogarithmic ($T = O(\text{polylog}(1/\varepsilon))$).

## When to reach for it

- Any adiabatic computation where the gap is bounded away from zero and you need high precision.
- As a drop-in replacement for [[Gap-Adapted Adiabatic Scheduling]] when precision matters more than raw $\kappa$-scaling: this trick trades slightly worse polylog factors in problem parameters for exponentially better precision dependence.
- The schedule is **universal** — it doesn't require knowledge of the gap or the condition number.

## Complexity

For the QLSP with condition number $\kappa$: runtime $T = O(\kappa \log^2(\kappa) \cdot \log^4(\log \kappa / \varepsilon))$.

The $\kappa$-dependence picks up polylog factors compared to [[Gap-Adapted Adiabatic Scheduling|gap-adapted scheduling]] ($O(\kappa/\varepsilon)$), but the precision dependence drops from $O(1/\varepsilon)$ to $O(\text{polylog}(1/\varepsilon))$.

## Caveat

- The polylog factors in $\kappa$ can be non-trivial. For moderate target precision, [[Gap-Adapted Adiabatic Scheduling]] with power-law schedules may have smaller total runtime.
- The superadiabatic analysis requires the Hamiltonian to be smooth in $s$ and the gap to remain open throughout the path — standard conditions but worth checking.
- The exponential decay exponent involves a fractional power ($T^{1/4}$ in the QLSP case), so the "exponential" improvement kicks in slowly.

## Related notes
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
- [[AQC-to-QAOA Parameter Transfer]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
