# Error Budget Optimization for Fault-Tolerant Algorithms

> **Source:** Reiher, Wiebe, Svore, Wecker, Troyer, arXiv:1605.03590 (Appendix E); now standard in fault-tolerant resource estimation
> **Tags:** #trick #error-analysis #resource-estimation #fault-tolerant #optimization

## What it does
Minimizes the total gate cost of a fault-tolerant algorithm by jointly optimizing the allocation of a fixed error budget across multiple error sources, rather than splitting it equally or ad hoc.

## The trick
In a typical fault-tolerant quantum simulation, the total error has $k$ independent sources:

$$\varepsilon_{\text{total}} = \varepsilon_1 + \varepsilon_2 + \cdots + \varepsilon_k \leq \varepsilon$$

Each $\varepsilon_i$ appears in the cost function with different scaling. For Reiher et al., the T-gate count depends on three error sources:

$$C = 2M \cdot \frac{\alpha}{\varepsilon_1} \cdot \beta\sqrt{\frac{\varepsilon}{\varepsilon_2}} \cdot \left(\gamma \log_2\left(\frac{2M}{\varepsilon_3}\beta\right) + \delta\right)$$

where $\varepsilon_1$ is [[Quantum Phase Estimation|QPE]] statistical error ($C \propto 1/\varepsilon_1$), $\varepsilon_2$ is [[Product Formula (Trotter-Suzuki)|Trotter]] systematic error ($C \propto 1/\sqrt{\varepsilon_2}$), and $\varepsilon_3$ is rotation synthesis error ($C \propto \log(1/\varepsilon_3)$).

Since these scale differently, the optimal allocation is *not* $\varepsilon/3$ each. The optimization:
1. Write $C(\varepsilon_1, \varepsilon_2, \varepsilon_3)$ subject to $\varepsilon_1 + \varepsilon_2 + \varepsilon_3 = \varepsilon$
2. Use calculus / Lagrange multipliers to minimize
3. Typically: give more budget to the source with the steepest cost dependence (QPE: $1/\varepsilon$) and less to the logarithmic one (synthesis: $\log 1/\varepsilon$)

For variance-based estimates, the constraint becomes $\varepsilon_{\text{TS}} + \sqrt{\varepsilon_{\text{QPE}}^2 + \varepsilon_{\text{Rot}}^2} \leq \varepsilon$, reflecting that statistical errors add in quadrature while systematic errors add linearly.

## When to reach for it
Any fault-tolerant resource estimation with multiple error sources. Now standard in:
- Chemistry resource estimates ([[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|THC]], [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|sparse qubitization]], [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|spectrum amplification]])
- Optimization resource estimates ([[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry et al. 2020]])
- Any QPE-based algorithm with Trotter or Taylor series simulation

## Complexity
The optimization itself is cheap — it's a low-dimensional constrained optimization, often solvable analytically. The payoff can be significant: for Reiher et al., the difference between naive equal splitting and optimized allocation is a constant factor improvement in total T count.

## Caveat
The optimization assumes you have good models for how cost scales with each $\varepsilon_i$. If those models are wrong (e.g., the Trotter error estimate is off by $10{,}000\times$, as was the case here), the "optimal" allocation is optimal for the wrong cost function. The framework is correct; the inputs may not be.

Also, for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based approaches, the Trotter error source disappears entirely, simplifying to a two-parameter optimization (QPE precision vs. Hamiltonian input model error).

## Related notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]
- [[Hybrid Classical-Quantum Chemistry Workflow]]
- [[Programmable Ancilla Rotations (PAR) for T-Depth Reduction]]
- [[Classical-Quantum Runtime Crossover Analysis]]
