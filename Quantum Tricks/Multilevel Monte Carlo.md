# Multilevel Monte Carlo

> **Source:** Giles (2008, 2015); used in An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283
> **Tags:** #trick #monte-carlo #SDE #variance-reduction

## What it does
Estimates an expectation using a hierarchy of increasingly accurate discretisations, spending many samples on cheap coarse levels and few samples on expensive fine levels.

## The trick
For approximations $P_0,P_1,\ldots,P_L$ to a target payoff $P$, use the telescoping identity

$$
\mathbb{E}[P_L]
=\mathbb{E}[P_0]+\sum_{\ell=1}^L \mathbb{E}[P_\ell-P_{\ell-1}].
$$

Couple $P_\ell$ and $P_{\ell-1}$ using the same underlying randomness, so the variance of the difference decays with $\ell$.

If

$$
V_\ell=\operatorname{Var}(P_\ell-P_{\ell-1}),\qquad C_\ell=\text{cost per level-}\ell\text{ sample},
$$

then allocate more samples to levels with high $V_\ell/C_\ell$ and fewer samples to expensive low-variance levels.

## Why it matters
For SDE simulation, plain Monte Carlo pays both sampling error and fine discretisation cost directly. MLMC removes most of the discretisation overhead. QA-MLMC then applies quantum mean estimation to each telescoping term.

## Caveat
The method depends on a good coupling. If $P_\ell-P_{\ell-1}$ does not have small variance, the hierarchy buys little.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum-Accelerated MLMC Error Allocation]]
