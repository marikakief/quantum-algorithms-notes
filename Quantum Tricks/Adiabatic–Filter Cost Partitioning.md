
> **Source:** Costa, An, Babbush, Berry, arXiv:2312.07690
> **Tags:** #trick #QLSP #resource-estimation #optimization #adiabatic #eigenstate-filtering

## What it does

Decomposes the total cost of a two-stage quantum algorithm (adiabatic preparation → [[Dolph-Chebyshev Eigenstate Filtering|eigenstate filtering]]) into its component costs and optimizes the intermediate error parameter $\Delta$ to minimize the total.

## The trick

The QLSP solver has two stages:
1. **Adiabatic step:** $T = \alpha\kappa/\Delta$ walk steps, producing a state with 2-norm error $\Delta$ (infidelity $\delta = \Delta^2 - \Delta^4/4$).
2. **Filtering step:** $\kappa\ln(2/\varepsilon_f)$ walk steps, boosting precision from the adiabatic overlap to final error $\varepsilon$.

Failed filtering triggers a retry of both stages. Accounting for retry probability and the [[Dolph-Chebyshev Eigenstate Filtering|LCU early-failure detection]] (which catches failures on average halfway through), the total expected cost is:

$$C_{\text{total}} = \frac{1}{2}\kappa\ln(2/\varepsilon_f)\left(\frac{1}{[1-\delta(1-\varepsilon_f^2)]^2} + 1\right) + \frac{\alpha\kappa}{\Delta[1-\delta(1-\varepsilon_f^2)]^2}$$

The parameter $\varepsilon_f$ (filter setting) relates to the final desired error $\varepsilon$ through the fidelity bound:

$$\varepsilon_f = \varepsilon \cdot \frac{\sqrt{(1-\delta)(1-\varepsilon^2/4)}}{(1-\varepsilon^2/2)\sqrt{\delta}}$$

Since $\kappa$ factors out, you can optimize $\Delta$ (equivalently $\delta$) for any given $\alpha$ and $\varepsilon$:
- For $\alpha \approx 1$: optimal $\Delta \approx 0.3$
- For $\alpha \approx 0.2$ (PD case): optimal $\Delta \approx 0.15$
- For $\alpha \approx 20$ (randomized method): optimal $\delta \approx 0.45$

The result: the adiabatic step dominates the cost for all practically relevant $\varepsilon$. The filtering step's $\ln(1/\varepsilon)$ contribution is visible but secondary.

## When to reach for it

- Any two-stage quantum algorithm with a "rough preparation" followed by "precision boost" structure.
- Resource estimation for the [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|optimal QLSP solver]].
- Comparing algorithms where one stage is cheap but inaccurate and the other is expensive but precise — the optimal split depends on the relative costs.
- Deciding how much effort to put into the adiabatic step vs. the filtering step when compiling circuits.

## Complexity

The optimization itself is a one-dimensional numerical optimization over $\Delta \in (0, \sqrt{2})$ for given $\alpha$ and $\varepsilon$. Trivially cheap classically.

## Caveat

The formula assumes the filtering success probability scales as analyzed in Costa et al. (2022). If the overlap from the adiabatic step is worse than $1-\delta$ (e.g., because the matrix has adversarial spectral structure), the retry rate increases and the optimal $\Delta$ shifts.

Also, the early-failure detection factor of $1/2$ assumes sequential LCU implementation with mid-circuit measurement. On architectures where mid-circuit measurement is expensive, the retry cost changes.

## Related notes
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[Dolph-Chebyshev Eigenstate Filtering]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
