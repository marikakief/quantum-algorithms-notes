# Adiabatic Error as Sum Over Non-Adiabatic Paths

> **Source:** Cheung, Høyer, Wiebe, arXiv:1103.4174 (builds on Ambainis-Regev arXiv:quant-ph/0411152 and Mackenzie-Marcotte-Paquette PRA 73:042104)
> **Tags:** #trick #adiabatic #path-integrals #error-analysis

## What it does

Gives an exact, non-perturbative expression for the adiabatic approximation error as a sum of path integrals over all "non-adiabatic paths" in the instantaneous eigenbasis. Makes the origin of adiabatic behaviour explicit: it's phase cancellation between paths, not just slowness of evolution.

## The trick

Discretise time into $L$ steps. At each step insert the resolution of identity in the instantaneous eigenbasis $\{|\nu(j/L)\rangle\}$. Expand the time-evolution operator into a sum over sequences of basis labels — each sequence is a **path**. In the limit $L \to \infty$:

$$U(1,0)|G(0)\rangle = e^{-i\int_0^1 E_G \cdot T}|G(1)\rangle + \sum_{q=1}^\infty \int_{J'_q} e^{-i\phi(\text{path})\cdot T} \prod_\ell \beta_{\nu_{\ell+1},\nu_\ell}(s_\ell) \cdot |\nu_q(1)\rangle \, d(J'_q)$$

where:
- $J'_q$ = set of all $q$-jump **non-adiabatic** paths (paths that start in $|G\rangle$ and end in some $|\nu \neq G\rangle$)
- $\beta_{\nu,\mu}(s) = \langle\nu(s)|\dot{H}(s)|\mu(s)\rangle / (E_\nu(s) - E_\mu(s))$ is the tunneling amplitude per unit dimensionless time
- $\phi(\text{path})$ is the dynamical phase accumulated along the path

The **error** is exactly this second term. The zero-jump path (staying in $|G\rangle$ throughout) is the adiabatic evolution itself.

**Why adiabaticity holds:** In the large-$T$ limit, the phase $e^{-i\phi\cdot T}$ oscillates rapidly for non-adiabatic paths. The contributions from paths jumping at different times interfere destructively, and the sum shrinks. See Fig. 2 in the paper for a graphic demonstration.

**Convergence fix (vs. MMP):** Mackenzie-Marcotte-Paquette (2006) had a similar path integral but summed over all paths including adiabatic ones, causing convergence problems. The fix: restrict to non-adiabatic paths only (those ending outside $|G(1)\rangle$). The adiabatic paths cancel internally and don't contribute to the error anyway.

## When to reach for it

- Proving adiabatic theorems: convert the error into path integral form, then bound each contribution.
- Understanding why higher-order derivatives of $H$ matter: $q$-jump paths contribute $\beta^q \sim (\|\dot{H}\|/\gamma)^q$, so multi-jump paths are suppressed only when $\|\dot{H}\|/\gamma \ll 1$.
- Identifying when the standard $O(1/T)$ estimate breaks: the Marzlin–Sanders counterexample has $\beta$ terms that don't decay fast enough with $T$, so the sum doesn't shrink.
- Any analysis of time-dependent quantum systems where you want to separate "adiabatic" from "non-adiabatic" contributions.

## Complexity

Each $q$-jump non-adiabatic path contributes $O((\|\dot{H}\|/\gamma_{\min})^q / T^q)$. Summing over all $q$ gives an exponential series that converges when $\|\dot{H}\| / (\gamma_{\min} T) \ll 1$.

## Caveat

Requires non-degeneracy throughout: $|G(s)\rangle$ must remain non-degenerate for all $s \in [0,1]$. Level crossings break the path decomposition. Works for bounded Hamiltonians on finite-dimensional spaces; extension to unbounded or infinite-dimensional cases requires additional work.

## Related notes
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Tunneling Amplitude Derivative Bound via Resolution of Identity]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
