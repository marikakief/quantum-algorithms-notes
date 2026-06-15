# Path-Dependent Payoffs by State Augmentation

> **Source:** An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283
> **Tags:** #trick #SDE #path-dependent-payoffs #option-pricing #state-augmentation

## What it does
Turns a path-dependent payoff into a terminal payoff of a higher-dimensional SDE, so terminal-value algorithms still apply.

## The trick
Suppose the payoff depends on terminal state and path integrals:

$$
P=P\!\left(X_T,\int_0^T f(X_t)\,dt,\int_0^T g(X_t)\,dW_t\right).
$$

Introduce auxiliary processes

$$
Y_t=\int_0^t f(X_s)\,ds,
\qquad
Z_t=\int_0^t g(X_s)\,dW_s.
$$

Then $(X_t,Y_t,Z_t)$ obeys an augmented SDE:

$$
dX_t=\mu(X_t,t)\,dt+\sigma(X_t,t)\,dW_t,
$$

$$
dY_t=f(X_t)\,dt,
\qquad
 dZ_t=g(X_t)\,dW_t.
$$

The original path-dependent payoff is now a terminal payoff $\widetilde P(X_T,Y_T,Z_T)$.

## When to reach for it
Use for Asian options, stochastic-integral estimators, time-averaged observables, and any Monte Carlo estimator whose output depends on the whole path but only through finitely many accumulated integrals.

## Complexity
The dimension and per-step arithmetic increase, but the MLMC/QA-MLMC error analysis can be reused if the augmented drift, volatility, and payoff satisfy the same regularity assumptions.

## Caveat
This is not free in high dimension. Extra path functionals increase register size, arithmetic cost, and regularity requirements.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]
