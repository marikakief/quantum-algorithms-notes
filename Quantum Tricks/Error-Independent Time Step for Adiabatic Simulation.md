
> **Source:** An, Costa, Berry, arXiv:2509.00171
> **Tags:** #trick #adiabatic #time-step #product-formulas #discrete-adiabatic

## What it does

In digitally simulated AQC, the time step size $h$ can be chosen independent of the tolerated error $\varepsilon$ and the total evolution time $T$. Only the Hamiltonian norms, commutator structure, and spectral gap determine $h$.

## The trick

For $H(s) = (1-f(s))H_0 + f(s)H_1$ with $\alpha = \|H_0\| + \|H_1\|$ and minimum gap $\Delta_*$:

**Exponential integrator:** Set $h = 1/\alpha$. Done.

**Simplified $p$-th order [[product formula]]:** Set
$$h = O\!\left(\min\!\left\{\frac{1}{\alpha},\; \frac{\Delta_*^{1/p}}{\tilde{\alpha}_p^{1/p}}\right\}\right)$$
where $\tilde{\alpha}_p = \sum_{\gamma_0,\ldots,\gamma_p \in \{0,1\}} \|[H_{\gamma_p}, \ldots, [H_{\gamma_1}, H_{\gamma_0}]]\|$.

To improve accuracy, increase $T$ (run the adiabatic evolution longer) rather than decreasing $h$. The total steps $T_d = T/h$ scale as $O(1/\varepsilon)$ rather than $O(1/\varepsilon^{1+1/p})$.

The reason: the sole purpose of choosing $h$ is to ensure the walk operator's spectral gap. Once the gap is established, the [[Discrete Adiabatic View of Numerical Integrators|discrete adiabatic framework]] handles the rest. The adiabatic error decreases with $T$, not with $h$.

**With boundary cancellation:** If $f^{(k)}(0) = f^{(k)}(1) = 0$ for all $k$, the step count drops to $O(1/\varepsilon^{1/k})$ for any $k$ — even with first-order methods. Set $h = O(1)$ (unit step size for bounded Hamiltonians).

## When to reach for it

- Digital AQC implementation where circuit depth is the bottleneck — use the largest viable step size.
- Resource estimation for adiabatic algorithms: the standard formula (which has $h$ depending on $\varepsilon$ and $T$) over-counts by a factor of $1/\varepsilon$ or more.
- Choosing between product-formula orders: the advantage of higher order comes from enabling larger $h$ (better gap matching), not from the standard $\varepsilon^{1/p}$ argument.

## Complexity

For first-order methods: $T_d = O(\alpha^3 \Delta_*^{-3} \varepsilon^{-1})$. Compared to $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-2})$ from standard analysis — saving a full factor of $1/\varepsilon$.

## Caveat

- The [[product formula]] must be the "simplified" version (scheduling function evaluated at a single time per step). Full higher-order Trotter-Suzuki with staggered time evaluations isn't covered by the current analysis.
- The gap-matching step size for product formulas can be small if commutators are large relative to the gap — the trick is most helpful when $\tilde{\alpha}_p$ is modest.
- Boundary cancellation results depend on Conjecture 10 (the high-order [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]]), which is numerically validated but not yet rigorously proven.

## Related notes
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
- [[Discrete Adiabatic View of Numerical Integrators]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Cross-Order Product Formula Threshold]]
- [[Gap-Adapted Adiabatic Scheduling]]
