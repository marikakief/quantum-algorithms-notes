# Symbolic Cost Propagation Through Call Graphs

> **Source:** Harrigan, Khattar, Yuan et al., arXiv:2409.04643
> **Tags:** #trick #resource-estimation #symbolic #optimization #software-pattern

## What it does
Produces closed-form algebraic expressions for quantum algorithm costs by propagating SymPy variables through a call graph, enabling analytical optimization over error budgets and algorithm parameters.

## The trick
Instead of computing numerical gate counts for fixed parameters, assign symbolic variables to key quantities (error tolerances $\Delta_{TS}$, $\Delta_{PE}$, $\Delta_{HT}$; system size $N$; interaction strength $U/t$; etc.) and let each bloq's cost formula be a SymPy expression. The call graph aggregation produces:

$$T_\text{cost} = \sum_{\text{bloq } b} n_b \cdot c_b(\text{params})$$

where $n_b$ is the (possibly symbolic) call count and $c_b$ is the symbolic cost. The result is an expression like Eq. (12) of the Qualtran paper:

$$T_\text{cost} = \frac{0.76\pi}{\Delta_{PE}} \cdot \left(\Delta_{TS}/\xi\right)^{-1/p} \cdot \left[\left(2L^2 + 6\lfloor L^2/2 \rfloor\right) \cdot \left\lceil 1.149 \log_2\left(\frac{N_R (\Delta_{TS}/\xi)^{-1/p}}{\Delta_{HT}}\right) + 9.2\right\rceil + 24\lfloor L^2/2 \rfloor\right]$$

for Trotterized phase estimation of the Hubbard model. This expression can then be numerically minimized over $(\Delta_{TS}, \Delta_{PE}, \Delta_{HT})$ subject to $\Delta_{TS} + \Delta_{PE} + \Delta_{HT} \le \varepsilon$.

**Why this matters:** Manual resource estimation papers typically fix an ad hoc error budget split (e.g., equal thirds) and compute a single number. Symbolic propagation lets you *find* the optimal split, which can differ from equal allocation by an order of magnitude in total cost.

## When to reach for it
- Multi-parameter error budgets where different components (Trotter error, phase estimation error, rotation synthesis error) trade off against each other
- Scaling analysis: want to know how cost depends on system size asymptotically *and* with the actual constant factors
- Comparing algorithms: the symbolic expressions make it transparent which regime favors which method

## Complexity
Classical overhead: SymPy expression manipulation. For most quantum algorithm call graphs, the symbolic expressions have $O(10)$–$O(100)$ terms and simplify in milliseconds. The numerical optimization step depends on the number of free parameters (typically 2–5, easily handled by grid search or `scipy.optimize`).

## Caveat
SymPy can struggle with quantum-information-specific functions like $\lceil\log_2 x\rceil$ that don't simplify cleanly. Floor/ceiling operations inside sums break many symbolic simplification rules. Sometimes you need to substitute numerical values for inner parameters while keeping outer ones symbolic.

## Related notes
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Hierarchical Bloq Decomposition for Scalable Resource Counting]]
- [[Error Budget Optimization via Symbolic Expressions]]
- [[Numerical Validation of Analytical Constant Factors]]
