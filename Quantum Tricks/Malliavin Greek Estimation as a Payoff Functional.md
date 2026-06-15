# Malliavin Greek Estimation as a Payoff Functional

> **Source:** An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283; Fournié et al. (1999)
> **Tags:** #trick #quantum-finance #Greeks #Malliavin-calculus #SDE #monte-carlo

## What it does
Represents a derivative of an option price as an expectation of a payoff-weighted stochastic integral, making Greeks accessible to the same quantum Monte Carlo estimator as prices.

## The trick
For a one-dimensional autonomous SDE

$$
dX_t=\mu(X_t)\,dt+\sigma(X_t)\,dW_t,
$$

introduce the variational process

$$
dY_t=\mu'(X_t)Y_t\,dt+\sigma'(X_t)Y_t\,dW_t,
\qquad Y_0=1.
$$

Under regularity assumptions, Delta can be written as

$$
\frac{\partial u(s,0)}{\partial s}
=
\mathbb{E}\!\left[
\frac{1}{T}P(X_T)\int_0^T Y_t\sigma^{-1}(X_t)\,dW_t
\mid X_0=s,Y_0=1
\right].
$$

So the Greek is just another scalar expectation. Simulate $(X_t,Y_t)$ and estimate this payoff with QA-MLMC.

## When to reach for it
Use when finite-difference Greeks are unstable, especially for nonsmooth payoffs, and when a Malliavin weight formula is available.

## Complexity
The paper inherits the QA-MLMC bounds: $\widetilde O(\epsilon^{-1})$ for strong-order $r>2$ in the piecewise-Lipschitz setting, and for $r\ge1$ in globally Lipschitz cases.

## Caveat
The formula needs stronger assumptions: differentiability of drift/volatility, nondegenerate volatility, and bounded moments. It is a way to reduce Greeks to expectation estimation, not a general automatic-differentiation theorem.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
