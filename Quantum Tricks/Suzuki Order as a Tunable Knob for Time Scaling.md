
> **Source:** Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139 (2005)
> **Tags:** #trick #product-formulas #suzuki #optimization

## What it does

Treats the Suzuki integrator order $2k$ as a free parameter to optimize, trading constant-factor overhead against the exponent of time scaling.

## The trick

For a $2k$-th order Suzuki integrator splitting $H = \sum_{j=1}^m H_j$ into $r$ time steps, the total number of exponentials scales as:

$$
N_{\mathrm{exp}} \sim m \cdot 5^{2k} \cdot \frac{(m\|H\|t)^{1+1/2k}}{\epsilon^{1/2k}}.
$$

Two competing effects as $k$ grows:

| $k$ increases | Time exponent $1+1/2k$ | Constant $5^{2k}$ |
|---|---|---|
| Better | $\to 1$ (linear) | Exponentially worse |

Optimizing $k \approx \frac{1}{2}\sqrt{\ln_5(m\|H\|t/\epsilon)}$ balances these, giving:

$$
N_{\mathrm{exp}} \leq 4m^2 \|H\|t \cdot \exp\!\bigl(2\sqrt{\ln 5 \cdot \ln(m\|H\|t/\epsilon)}\bigr).
$$

The time dependence is essentially linear with a subexponential correction — much better than any fixed-order formula, but not truly optimal ([[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] later achieves $O(\|H\|t + \log(1/\epsilon))$).

## When to use this reasoning

- Analyzing [[Order-Condition Cancellation in Product Formulas|product-formula]] costs: don't commit to a fixed Suzuki order. Treat $k$ as a variable.
- Comparing product-formula approaches against [[Linear Combination of Unitaries (LCU)|LCU]]/[[QSVT Meta-Template|QSP]]: this gives the best [[Product Formulas]]s can do asymptotically.
- The same "order as knob" idea appears whenever a family of approximants is parameterized by degree/order (Taylor series truncation, polynomial approximation degree in [[QSVT Meta-Template|QSP]], etc.).

## Caveat

The $5^{2k}$ blowup comes from the Suzuki recursion structure (each level multiplies the number of exponentials by 5). This is a real cost, not just a bookkeeping artifact. For practical system sizes and precisions, the optimal $k$ is usually small (2–4), and the "near-linear" regime only kicks in at large $\tau$.

## Related notes

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — extends this trick to time-dependent Hamiltonians; same order optimization, but order is now capped by $H(t)$'s smoothness
- [[Order-Condition Cancellation in Product Formulas]]
- [[Trotter Commutator-Scaling Bound]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
