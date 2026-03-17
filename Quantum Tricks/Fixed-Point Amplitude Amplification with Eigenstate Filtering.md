
> **Tags:** #trick #amplitude-amplification #state-preparation #robust
> **Source:** arXiv:2510.04929 (Quantum Hermite Transform)

## What it does
Boosts overlap with a target eigenspace to $1-\varepsilon$ **without knowing the exact initial overlap**.

## The trick
1. Use an eigenstate filter (via QPE + phase windowing) to map
   $$|\phi\rangle \mapsto \beta |\psi_{\text{target}}\rangle|0\rangle + |\perp\rangle$$
   with unknown but constant $\beta$.
2. Apply fixed-point amplitude amplification polynomial (Yoder–Low–Chuang / Gilyén et al.) instead of standard Grover rotation.
3. The fixed-point polynomial monotonically suppresses failure amplitude, avoiding overshoot even when $\beta$ is unknown.

## When to reach for it
- You can get constant overlap with target subspace, but overlap varies by instance/state index
- Standard amplitude amplification is unstable due to unknown angle
- You need uniform guarantees over a superposition of labels (like all $n$ in QHT)

## Complexity
Adds a multiplicative $O(\log(1/\varepsilon))$ overhead on top of the filtering circuit. The fixed-point polynomial (due to Yoder, Low, Chuang 2014; generalized in [[QSVT Meta-Template|QSVT]] framework) has degree $O(\log(1/\varepsilon)/\gamma)$ where $\gamma$ is the initial overlap — but the point is you don't need to know $\gamma$ in advance, just that $\gamma \ge \gamma_\text{min} > 0$.

## Caveat
Requires a known lower bound on initial overlap $\gamma_\text{min} > 0$. If overlap can be exponentially small in the system size (which can happen for highly excited states), no rescue — you either need a better initial state or accept exponential overhead. In the QHT context, the Plancherel-Rotach approximation guarantees constant overlap uniformly over all $n$, which is exactly what makes this step work.
