# Quantum-Accelerated MLMC Error Allocation

> **Source:** An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283
> **Tags:** #trick #multilevel-monte-carlo #amplitude-estimation #SDE

## What it does
Applies quantum mean estimation to each level of a multilevel Monte Carlo telescoping sum without losing the MLMC variance/cost balancing.

## The trick
Write the target as

$$
\mathbb{E}[P_L]=\sum_{\ell=0}^{L}\mathbb{E}[P_\ell-P_{\ell-1}],\qquad P_{-1}=0.
$$

For each level, estimate $\mathbb{E}[P_\ell-P_{\ell-1}]$ using [[Quantum Mean Estimation via Amplitude Estimation]]. If

$$
V_\ell=O(2^{-\beta\ell}),\qquad C_\ell=O(2^{\gamma\ell}),
$$

then quantum estimation cost per level is controlled by $2^{-\widehat\beta\ell}/\epsilon_\ell$ with $\widehat\beta=\beta/2$, rather than the classical $2^{-\beta\ell}/\epsilon_\ell^2$.

Choose $L=\lceil \log(2/\epsilon)/\alpha\rceil$ to control bias, and allocate $\epsilon_\ell$ so that $\sum_\ell \epsilon_\ell\le \epsilon/2$ while favouring levels with better variance/cost tradeoff.

## When to reach for it
Use when a classical MLMC algorithm already has a good hierarchy: cheap high-variance coarse levels and expensive low-variance fine levels. The trick is most useful when the output is a scalar expectation.

## Complexity
The clean regime is $\widehat\beta>\gamma$, giving

$$
\widetilde O(\epsilon^{-1})
$$

up to the cost of one level sample. If $\widehat\beta<\gamma$, the fine levels still dominate and the exponent worsens to $1+(\gamma-\widehat\beta)/\alpha$.

## Caveat
The hierarchy must have reversible/coherent level samplers. Also, if variance decay is too slow relative to level cost, quantum mean estimation cannot fully remove the fine-level overhead.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
