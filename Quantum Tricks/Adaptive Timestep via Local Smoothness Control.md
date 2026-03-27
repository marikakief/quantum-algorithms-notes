
> **Source:** Wiebe, Berry, Høyer & Sanders, arXiv:1011.3489 (2011)
> **Tags:** #trick #adaptive #hamiltonian-simulation #time-dependent #product-formulas

## What it does

Replaces the global worst-case smoothness parameter $\Lambda$ with a time-varying bound $\Upsilon(t)$ (the "pointwise-smooth parametrization"), choosing step sizes adaptively so the local Suzuki error is within budget on each sub-interval. The total simulation cost then scales with $\bar\Upsilon$ — the time-average of $\Upsilon$ — rather than $\Lambda_{\max}$, giving a speedup whenever $\|H(t)\|$ is unevenly distributed over time.

## The trick

Define $\Upsilon(t) \geq \left(\sum_\mu \|H_\mu^{(p)}(t)\|\right)^{1/(p+1)}$ for all $p \in \{0,\ldots,2k\}$ — a local analogue of the [[Λ-Smoothness Parametrization|global $\Lambda$ bound]], but evaluated pointwise.

Assume $|\partial_t \Upsilon(t)| \leq K^2[\Upsilon(t)]^2$ — this controls how fast $\Upsilon$ can change (Lipschitz condition on $1/\Upsilon$).

Choose subintervals $[t_p, t_{p+1}]$ such that

$$
\max_{t\in[t_p,t_{p+1}]}\Upsilon(t)\cdot(t_{p+1}-t_p) \leq \frac{(\varepsilon/r)^{1/(2k+1)}}{24d^2k(5/3)^{k-1}}.
$$

Apply the order-$k$ Suzuki formula on each subinterval. The total query cost (Theorem 8) is

$$
\text{QueryCost} \leq 12CMd^2 5^{k-1}\left\lceil\left[24d^2k\left(\frac{5}{3}\right)^{k-1}\bar\Upsilon\Delta t\right]^{1+1/(2k)}(\varepsilon/4)^{-1/(2k)} + 3K^2\bar\Upsilon\Delta t + 1\right\rceil,
$$

where $\bar\Upsilon = \frac{1}{\Delta t}\int_{t_0}^{t_0+\Delta t}\Upsilon(t)\,dt$.

The constant-step cost (Theorem 5) uses $\Lambda_{\max}$ in place of $\bar\Upsilon$. Since $\bar\Upsilon \leq \Lambda_{\max}$ always, this is never worse and can be substantially better.

## When to reach for it

- $H(t)$ has bursts of high norm concentrated in short time windows
- Time-dependent perturbations that are large at $t = 0$ but decay rapidly
- Any setting where using a fixed step size wastes computation on "quiet" periods

## Complexity

Scales with $\bar\Upsilon$ rather than $\Lambda_{\max}$. If $\|H(t)\|$ is concentrated in a fraction $f$ of the total interval with norm $\Lambda_{\max}$ and near-zero elsewhere, $\bar\Upsilon \approx f\Lambda_{\max}$, giving an $f$-fold improvement.

The $3K^2\bar\Upsilon\Delta t$ term captures the extra cost of adapting the step size: more steps are needed in high-$\Upsilon$ regions, and the $K^2$ factor penalizes rapidly changing $\Upsilon$.

## Caveat

The regularity condition $|\partial_t\Upsilon| \leq K^2\Upsilon^2$ must hold. It fails for Hamiltonians with instantaneous norm spikes (step-function-like $\|H(t)\|$) — for those, use the [[Discontinuity Excision for Hamiltonian Simulation]] approach instead.

## Related notes
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Λ-Smoothness Parametrization]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Discontinuity Excision for Hamiltonian Simulation]]
