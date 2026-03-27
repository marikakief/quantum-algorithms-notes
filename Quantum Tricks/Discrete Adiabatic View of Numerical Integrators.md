
> **Source:** An, Costa, Berry, arXiv:2509.00171
> **Tags:** #trick #adiabatic #discrete-adiabatic #product-formulas #error-analysis

## What it does

Reframes time-discretisation of adiabatic quantum dynamics: instead of bounding continuous adiabatic error + numerical discretisation error separately, treat each numerical propagator as a walk operator and apply the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] directly to the whole sequence.

## The trick

Standard analysis bounds:
$$\text{total error} \leq \underbrace{O(\alpha^2/(\Delta_*^3 T))}_{\text{continuous adiabatic}} + \underbrace{O(h^p T)}_{\text{discretisation}}$$

These two terms pull in opposite directions: smaller $h$ reduces discretisation error but doesn't help the adiabatic term. The standard approach optimises $h$ given $T$ and $\varepsilon$.

The new view: let $W(j/T_d) = U_{\text{num}}((j+1)h, jh)$ be the numerical propagator at step $j$. Verify that:
1. $W(s)$ varies slowly: $\|D^{(k)} W(s)\| \leq c_k(s)/T_d^k$. This is automatic because $H(t/T)$ varies on the slow scale $s = t/T$.
2. $W(s)$ has a spectral gap. For the exponential integrator $e^{-ihH(s)}$ with $h \leq 1/\alpha$, the gap is $h\Delta_*$. For [[product formula]]s, reduce $h$ until the [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Trotter error]] is small enough to preserve the gap.
3. The final walk operator's eigenstate matches the target.

Then the discrete adiabatic theorem gives the total error directly — one bound, not a sum of two.

The error bound from Lemma 2 (the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]]) involves $c_1(s)/(\Delta^2 T_d)$ boundary terms and $c_1^2/(\Delta^3 T_d)$ bulk sums. Since $c_1 = O(h\alpha)$ and the gap scales as $h\Delta_*$, the $h$ cancels in the ratio and you get a bound $O(\alpha^2/(\Delta_*^3 T))$ — independent of $h$.

## When to reach for it

- Any digital implementation of adiabatic quantum computing where you want to avoid over-refining the time step.
- Analysing product-formula-based AQC: the standard Trotter error bounds (which scale with $T$) give the wrong picture for adiabatic dynamics.
- When the Hamiltonian gap is well-characterised but you want to use a simple (first-order) integrator.
- Understanding why first-order Trotter with large step size works surprisingly well for adiabatic problems.

## Complexity

No extra gates. The trick is purely analytical — it changes how you set $h$ and $T$, typically resulting in fewer total steps than standard analysis recommends. For the exponential integrator, the improvement is a factor of $1/\varepsilon$ in total steps.

## Caveat

The walk operators must satisfy the gap condition. For the exponential integrator this is straightforward; for [[product formula]]s at large $h$, the gap of the walk operator can differ from the Hamiltonian's gap in unpredictable ways (see [[Trotter Gap Independence from Hamiltonian Gap]]). The "simplified" product formula (all scheduling evaluations at the same time point) is needed for a clean analysis — the full higher-order Trotter-Suzuki formula with staggered evaluations doesn't fit directly.

## Related notes
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Error-Independent Time Step for Adiabatic Simulation]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
