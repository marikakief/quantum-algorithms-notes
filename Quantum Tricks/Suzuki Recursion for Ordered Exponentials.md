
> **Source:** Wiebe, Berry, Høyer, Sanders, arXiv:0812.0562 (J. Phys. A 2010)
> **Tags:** #trick #product-formulas #suzuki #time-dependent #ordered-exponential

## What it does

Extends the Suzuki recursive [[product formula]] from the time-independent case to ordered operator exponentials $\mathcal{T}\exp\!\int_\mu^\lambda H(u)\,du$, achieving error $O(\Delta\lambda^{2k+1})$ at order $k$ — the same scaling as time-independent Suzuki — provided $H(u)$ has $2k$ bounded derivatives.

## The trick

**Step 1 — Base integrator.** For a step $[\mu, \mu+\Delta\lambda]$, define the symmetric base formula by evaluating all terms at the midpoint:

$$
U_1(\mu+\Delta\lambda, \mu) = \left(\prod_{j=1}^m e^{H_j(\mu+\frac{\Delta\lambda}{2})\frac{\Delta\lambda}{2}}\right)\left(\prod_{j=m}^1 e^{H_j(\mu+\frac{\Delta\lambda}{2})\frac{\Delta\lambda}{2}}\right).
$$

Error is $O(\Delta\lambda^3)$: time-dependence doesn't add extra cost at this order.

**Step 2 — Recursion.** Given symmetric $U_p$ with error $O(\Delta\lambda^{2p+1})$, define $U_{p+1}$ as a product of five $U_p$ evaluations at subintervals of sizes $s_p\Delta\lambda,\, s_p\Delta\lambda,\, (1-4s_p)\Delta\lambda,\, s_p\Delta\lambda,\, s_p\Delta\lambda$ where

$$
s_p = \frac{1}{4 - 4^{1/(2p+1)}}.
$$

The key point: the subintervals are chosen to preserve the ordering structure of $U(\lambda,\mu)$. Each $U_p$ application covers a contiguous ordered interval; their composition correctly chains together the time-ordered segments. This is what makes the recursion valid for ordered exponentials, not just for $e^{tH}$.

**Step 3 — Error cancellation.** The specific choice of $s_p$ cancels the leading-order error term in $U_p$ in a symmetric way, making $U_{p+1}$ accurate to $O(\Delta\lambda^{2p+3})$. The proof uses Taylor expansion of the ordered exponential via the recursive derivatives $\partial_\lambda^p U = T_p(\lambda)U(\lambda,\mu)$ (where $T_{p+1}(\lambda) = T_p(\lambda)H(\lambda) + \partial_\lambda T_p(\lambda)$, $T_0 = I$) plus symmetry arguments.

## When to reach for it

- Time-dependent [[Hamiltonian simulation]] with smooth $H(t)$.
- Any ordered-operator problem where you need explicit error bounds and $H$ is not analytic (Dyson-series methods need stronger assumptions on convergence; this works with finite smoothness).
- When you want to optimize Suzuki order $k$ to get near-linear scaling in evolution time — the [[Suzuki Order as a Tunable Knob for Time Scaling|order-as-knob]] strategy carries over unchanged from the time-independent case.

## Complexity

$k$-th order formula uses at most $2m \cdot 5^{k-1}$ exponentials per step. For total accuracy $\varepsilon$ over interval $\Delta\lambda$, with $\Lambda$–$2k$-smooth $H$:

$$
N \leq 2m\cdot 5^{k-1}\left\lceil 5^{k}\Lambda\Delta\lambda\left(\frac{(5/3)^k\Lambda\Delta\lambda}{\varepsilon}\right)^{1/(2k)}\right\rceil.
$$

For $\infty$-smooth $H$, optimize $k \approx \sqrt{\frac{1}{2}\log_{25/3}(\Lambda\Delta\lambda/\varepsilon)}$ to get near-linear $N$ in $\Delta\lambda$.

## Caveat

Only works if $H(u)$ has $2k$ bounded derivatives. If $H$ has only $P$ derivatives, the recursion doesn't improve beyond order $P$ — see [[Smoothness-Order Saturation for Product Formulas]]. The $5^{k-1}$ exponential blowup is real, so the near-linear regime only kicks in at large $\Lambda\Delta\lambda/\varepsilon$.

## Related notes
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Λ-Smoothness Parametrization]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
