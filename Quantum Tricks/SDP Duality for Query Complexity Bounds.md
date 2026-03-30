# SDP Duality for Query Complexity Bounds

> **Source:** Špalek and Szegedy, arXiv:quant-ph/0409116 (2006); also Barnum-Saks-Szegedy (2003)
> **Tags:** #trick #query-complexity #semidefinite-programming #lower-bounds #adversary-method

## What it does
Converts between primal (adversary matrix) and dual (distribution/SDP) formulations of quantum query lower bounds, showing they yield the same value and enabling different proof strategies for different problems.

## The trick

In the quantum adversary framework, the **primal** formulation constructs a non-negative symmetric adversary matrix $\Gamma$ with $\Gamma[x,y] = 0$ whenever $f(x) = f(y)$, and the bound is:

$$\text{Adv}(f) = \max_\Gamma \frac{\lambda(\Gamma)}{\max_i \lambda(\Gamma \circ D_i)}$$

The **dual** formulation assigns probability distributions $p_x$ over input variables to each input $x$:

$$\text{Adv}(f) = \frac{1}{\max_p \min_{\substack{x,y \\ f(x) \neq f(y)}} \sum_{i: x_i \neq y_i} \sqrt{p_x(i) \cdot p_y(i)}}$$

These are connected through the SDP:

**Primal (SMM):** Maximize $\mu$ subject to $R_i \succeq 0$, $\sum_i R_i \circ I = I$, $\sum_i R_i \circ D_i \geq \mu F$

**Dual (GSA):** Minimize $\text{tr}(\Delta)$ subject to $\Delta$ diagonal, $Z \geq 0$, $Z \cdot F = 1$, $\forall i: \Delta - Z \circ D_i \succeq 0$

Strong duality holds (the SDP is strictly feasible), so primal optimum = dual optimum.

The key insight: any rank-1 relaxation in the SDP can be reduced back to rank-1 via Cholesky decomposition + Cauchy-Schwarz, so the SDP relaxation is tight.

## When to reach for it

- When the primal adversary matrix is hard to construct but a good distribution over variables is easy to find (or vice versa)
- When you want to prove *limitations* of the adversary method — the dual/distribution formulation makes the [[Certificate Complexity Barrier for Adversary Methods|certificate complexity barrier]] transparent
- When connecting adversary bounds to other SDP-based tools in quantum information (e.g., the Barnum-Saks-Szegedy characterisation of query complexity)

## Complexity
No computational overhead — this is a proof technique. Converting between formulations may simplify the optimisation problem from an exponential search (over $\Gamma$ matrices) to a structured SDP.

## Caveat
This applies only to the *positive-weight* adversary method, which is bounded by $\sqrt{C_0 C_1}$. The negative-weight adversary (Høyer-Lee-Špalek 2007) also has an SDP formulation but lifts the non-negativity constraint on $\Gamma$, making it strictly stronger and, in fact, tight for all Boolean functions.

## Related notes
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Certificate Complexity Barrier for Adversary Methods]]
- [[Hadamard Product Spectral Norm Bound]]
- [[Matrix Multiplicative Weights Update Method for SDPs]]
