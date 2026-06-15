# Sublinear Binomial Sampling for Quantum Option Pricing

> **Source:** An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283; Bringmann et al. (2014); Farach-Colton--Tsai (2015)
> **Tags:** #trick #quantum-finance #binomial-sampling #option-pricing #amplitude-estimation

## What it does
Avoids explicitly walking a binomial option-pricing tree when the payoff can be evaluated from the number of up-steps.

## The trick
In a binomial lattice model,

$$
S_n = U^kD^{n-k}S_0,
\qquad k\sim \operatorname{Bin}(n,p).
$$

For piecewise-constant payoffs, precompute the threshold values $k_j^*$ satisfying

$$
k_j^*\log U+(n-k_j^*)\log D+\log S_0=\log S_j.
$$

Then evaluating the payoff only requires sampling $k\sim\operatorname{Bin}(n,p)$ and comparing $k$ with the thresholds. Fast binomial samplers do this in $O(\log n)$ expected time rather than $O(n)$ tree steps.

The quantum version uses a coherent payoff oracle

$$
U_\psi|S_0\rangle|k\rangle|0\rangle
=|S_0\rangle|k\rangle|\psi(U^kD^{n-k}S_0)\rangle,
$$

then applies [[Quantum Mean Estimation via Amplitude Estimation]].

## When to reach for it
Use for lattice models where the terminal value depends on a simple sufficient statistic, especially binomial counts, and the payoff is threshold-based.

## Complexity
For $n=O(\epsilon^{-2})$ lattice steps, classical sampling gives $\widetilde O(\epsilon^{-2})$ expectation estimation; quantum mean estimation gives

$$
\widetilde O(\epsilon^{-1}).
$$

## Caveat
The trick relies on payoff structure. If the payoff depends on the full path, not just terminal count, the binomial-count shortcut no longer applies.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
