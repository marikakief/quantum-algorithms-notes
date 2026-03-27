
> **Source:** Cheung, Høyer, Wiebe, arXiv:1103.4174 (Lemma 3 and Appendix A)
> **Tags:** #trick #adiabatic #operator-bounds #resolution-of-identity #eigenstate-derivatives

## What it does

Bounds the first and second derivatives of the tunneling amplitude $\beta_{\nu,\mu}(s) = \langle\nu|\dot H|\mu\rangle / \gamma_{\nu\mu}$ in terms of $\|\dot H\|$, $\|\ddot H\|$, $\|\dddot H\|$, and $\gamma_{\min}$ only — no factors of the Hilbert space dimension $N$. This is essential for making adiabatic error bounds dimension-independent.

## The trick

The naive approach: differentiate $\beta_{\nu,\mu} = \langle\nu|\dot H|\mu\rangle / \gamma_{\nu\mu}$ and bound term-by-term. Problem: terms like $\langle\dot\nu|\dot H|\mu\rangle$ involve eigenstate derivatives, which sum to $N$ terms via $|\dot\nu\rangle = -\sum_m |m\rangle\beta_{m,\nu}$. Bounding this sum naively gives factors of $N$.

**The fix:** Instead of bounding each term separately, sum over all state indices first, using the resolution of identity $\sum_m |m(s)\rangle\langle m(s')| = $ operator of unit norm for any $s, s'$.

Concretely, to bound $\|\sum_{\nu_q, \ldots, \nu_1} \chi(\vec\nu)|\nu_q(s')\rangle \dot\beta_{\nu_q,\nu_{q-1}}(s)\|$:

1. Write $|\dot\nu\rangle = -\sum_m |m\rangle\beta_{m,\nu}$ (geometric-phase gauge)
2. Rewrite $\dot\beta$ entirely in terms of matrix elements $\langle\nu|\dot H|m\rangle$, $\langle\nu|\ddot H|\mu\rangle$, and $\dot\gamma$ — no eigenstate derivatives remain
3. Each factor $\sum_m |m(s')\rangle\langle m(s)|$ has operator norm 1 regardless of $N$
4. Apply triangle inequality and bound $|\dot\gamma_{\nu\mu}| \leq 2\|\dot H\|$

Result (Lemma 3):

$$\left\|\sum_{\nu_q,\ldots} \chi(\vec\nu)|\nu_q(s')\rangle\dot\beta_{\nu_q,\nu_{q-1}}(s)\right\| \leq \left(\frac{4\|\dot H\|^2}{\gamma_{\min}^2} + \frac{\|\ddot H\|}{\gamma_{\min}}\right)\left\|\sum_{\nu_{q-1},\ldots} \chi(\vec\nu)|\nu_{q-1}(s)\rangle\right\|$$

For second derivatives, an analogous bound involves $\|\dot H\|$, $\|\ddot H\|$, $\|\dddot H\|$, and the bound $|\ddot\gamma| \leq 2\|\ddot H\| + 8\|\dot H\|^2/\gamma_{\min}$.

## When to reach for it

- Bounding multi-jump path contributions in adiabatic error analysis (the Cheung–Høyer–Wiebe application).
- Any context where you're differentiating quantities involving eigenstates and want dimension-independent bounds.
- Analysis of Lindblad or QLSP evolution where tunneling amplitudes appear in perturbative expansions.

## Complexity

The bound on $\|\dot\beta\|$ is $O(\|\dot H\|^2/\gamma_{\min}^2 + \|\ddot H\|/\gamma_{\min})$. For $\|\ddot\beta\|$: $O(\|\dot H\|^3/\gamma_{\min}^3 + \|\dot H\|\|\ddot H\|/\gamma_{\min}^2 + \|\dddot H\|/\gamma_{\min})$.

## Caveat

The gauge choice $\langle\dot\nu|\nu\rangle = 0$ (Berry/geometric phase absorbed) is necessary for the simplification $|\dot\nu\rangle = -\sum_{m\neq\nu}|m\rangle\beta_{m,\nu}$. In other gauges the bookkeeping is messier but the final bounds are the same.

## Related notes
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Adiabatic Error as Sum Over Non-Adiabatic Paths]]
- [[Geometric-Phase-Cancelling Eigenstate Gauge Choice]]
