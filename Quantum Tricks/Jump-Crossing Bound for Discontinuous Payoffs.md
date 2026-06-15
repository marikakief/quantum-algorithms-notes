# Jump-Crossing Bound for Discontinuous Payoffs

> **Source:** An--Linden--Liu--Montanaro--Shao--Wang, arXiv:2012.06283; Giles--Higham--Mao (2009)
> **Tags:** #trick #SDE #payoff-functions #error-analysis #multilevel-monte-carlo

## What it does
Controls discretisation error for piecewise-Lipschitz payoffs by separating ordinary Lipschitz error from the probability that the numerical path crosses a payoff discontinuity.

## The trick
For a payoff $P$ with jumps at $l_1,\ldots,l_q$, compare the true terminal value $X_T$ and the numerical approximation $\widehat X_n$ along the segment

$$
\Lambda(\lambda)=\lambda \widehat X_n+(1-\lambda)X_T.
$$

Let $M(\widehat X_n,X_T)$ count how many discontinuities this segment crosses. Then

$$
\mathbb{E}|P(\widehat X_n)-P(X_T)|
\le L\mathbb{E}|\widehat X_n-X_T|+qJ\Pr[M(\widehat X_n,X_T)\ge 1],
$$

where $J$ bounds jump size.

The crossing probability is bounded by

$$
\Pr[M\ge 1]
\le \Pr\!\left[\min_j |X_T-l_j|\le h^a\right]
+\Pr\!\left[|\widehat X_n-X_T|\ge h^a\right].
$$

A bounded density for $X_T$ handles the first term; Markov's inequality plus strong convergence handles the second.

## When to reach for it
Use for option-style payoffs with thresholds: digital options, barrier indicators, clipped rewards, or piecewise-constant functions of a stochastic process.

## Complexity
For a strong-order-$r$ scheme, the paper obtains

$$
\alpha=r-o(1),\qquad \beta=r-o(1)
$$

for piecewise-Lipschitz payoffs, rather than the globally Lipschitz values $\alpha=r$ and $\beta=2r$.

## Caveat
The bound is conservative and depends on density assumptions near the discontinuities. It also explains why discontinuous payoffs demand higher-order schemes in QA-MLMC.

## Related notes
- [[Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes]]
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]
