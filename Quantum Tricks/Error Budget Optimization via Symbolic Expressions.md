# Error Budget Optimization via Symbolic Expressions

> **Source:** Harrigan, Khattar, Yuan et al., arXiv:2409.04643; also used in Kivlichan, Gidney, Babbush et al. (2020) and Campbell (2021)
> **Tags:** #trick #resource-estimation #optimization #error-budget #compilation

## What it does
Finds the minimum-cost allocation of a total error budget $\varepsilon$ across multiple error sources (Trotter error, phase estimation error, rotation synthesis error, etc.) by generating a symbolic cost expression and numerically optimizing over the allocation.

## The trick
Most fault-tolerant quantum algorithms have a total error bound of the form:

$$\varepsilon \le \Delta_1 + \Delta_2 + \cdots + \Delta_k$$

where each $\Delta_i$ is a controllable error source. The total gate cost $C(\Delta_1, \ldots, \Delta_k)$ is a decreasing function of each $\Delta_i$ (tighter error → more gates), but the dependence is different for each source. For example, in Trotterized phase estimation:

- Trotter error $\Delta_{TS}$: cost scales as $(\Delta_{TS})^{-1/p}$ where $p$ is the Trotter order
- Phase estimation error $\Delta_{PE}$: cost scales as $1/\Delta_{PE}$
- Rotation synthesis error $\Delta_{HT}$: cost scales as $\log(1/\Delta_{HT})$

Equal allocation ($\Delta_i = \varepsilon/k$) is suboptimal because rotation synthesis has logarithmic sensitivity — it should be given a much tighter budget than the other two. The optimal allocation depends on the specific problem parameters.

**Process:**
1. Generate the symbolic cost expression via [[Symbolic Cost Propagation Through Call Graphs|symbolic call-graph propagation]]
2. Substitute concrete problem parameters (system size, interaction strengths, etc.)
3. Numerically minimize over $(\Delta_1, \ldots, \Delta_k)$ subject to $\sum_i \Delta_i \le \varepsilon$

The Qualtran paper demonstrates this for the Hubbard model (Eq. 12, Fig. 14), showing contour plots of constant cost over the $(\Delta_{TS}, \Delta_{PE})$ plane.

## When to reach for it
- Any resource estimation with $\ge 2$ independent error sources
- Especially when different error sources have different functional dependences on their budget (polynomial vs logarithmic vs exponential)
- When comparing algorithms: the optimal allocation can change which algorithm "wins" compared to naive equal allocation

## Complexity
The optimization is over a small number of continuous variables ($k = 2$–5 typically) with a smooth objective function. Grid search, `scipy.optimize.minimize`, or even manual exploration of contour plots suffices. The bottleneck is generating the symbolic expression, not optimizing it.

## Caveat
The error decomposition $\varepsilon \le \sum_i \Delta_i$ is usually a triangle inequality bound. The actual error may be smaller — correlated error cancellations can make the real error significantly below the sum. Optimizing the budget allocation based on worst-case bounds may over-allocate to sources whose actual contribution is smaller. Tighter error analysis (e.g., commutator bounds for Trotter error) can change the optimal allocation.

## Related notes
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Symbolic Cost Propagation Through Call Graphs]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Trotterized Hubbard with explicit error budget optimization
- [[Extensive vs Intensive Error Targeting]]
- [[Numerical Validation of Analytical Constant Factors]]
