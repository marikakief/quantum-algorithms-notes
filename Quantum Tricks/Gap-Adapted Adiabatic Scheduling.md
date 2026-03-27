
> **Source:** Dong An and Lin Lin, arXiv:1909.05500; builds on Roland and Cerf, arXiv:quant-ph/0107015
> **Tags:** #trick #adiabatic #scheduling #spectral-gap #eigenpath-tracking

## What it does

Optimises the interpolation speed in adiabatic quantum computing by making the schedule track the instantaneous spectral gap, slowing down where the gap is small and speeding up where it's large.

## The trick

In standard AQC with $H(s) = (1-f(s))H_0 + f(s)H_1$, the adiabatic theorem demands slow evolution through spectral gap bottlenecks. Instead of a uniform schedule $f(s) = s$, define $f(s)$ via:

$$f'(s) = c_p \, \Delta(f(s))^p, \quad f(0) = 0,$$

where $\Delta(f)$ is a lower bound on the spectral gap at interpolation parameter $f$, $p > 0$ is a tuning exponent, and $c_p$ normalises so $f(1) = 1$.

The schedule automatically concentrates evolution time near the gap minimum. For $1 < p < 2$, the total runtime scales as $O(1/\Delta_{\min})$ rather than $O(1/\Delta_{\min}^2)$ that a naive global adiabatic condition would give.

[[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland and Cerf (2002)]] introduced the $p = 2$ case for Grover search, recovering $O(\sqrt{N})$. [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An and Lin (2019)]] generalised it to $1 < p < 2$ for QLSP, removing a $\log \kappa$ factor.

## When to reach for it

- Any eigenpath-tracking problem where you have an analytic or computable lower bound on the spectral gap as a function of the interpolation parameter.
- Adiabatic state preparation where the gap profile is non-uniform (which is almost always).
- As a principled initialisation for variational/QAOA-type circuits (see [[AQC-to-QAOA Parameter Transfer]]).

## Complexity

The runtime improvement depends on the gap profile. For QLSP with gap $\Delta(f) \geq 1 - f + f/\kappa$:
- $p = 1$ or $p = 2$: $T = O(\kappa \log \kappa / \varepsilon)$
- $1 < p < 2$: $T = O(\kappa / \varepsilon)$ — linear in $\kappa$, matching the lower bound up to precision factors.

## Caveat

Requires a **computable lower bound** on the spectral gap as a function of $f$. If the gap is unknown or hard to bound analytically, the schedule can't be constructed a priori. For problems where the gap structure is complicated (e.g., random instances of optimisation problems), this trick may not apply directly.

The $1/\varepsilon$ dependence is still polynomial. For polylogarithmic precision, combine with [[Superadiabatic Smooth Scheduling for Exponential Precision]].

## Related notes
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
- [[AQC-to-QAOA Parameter Transfer]]
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — Extends gap-adapted scheduling to the discretised setting with $h = 1$; adds boundary cancellation variant
- [[Boundary-Cancellation Schedule for Adiabatic Grover Search]]
