
> **Source:** Cheung, Høyer, Wiebe, arXiv:1103.4174; standard technique — see also Berry (1984), Wilczek-Zee (1984)
> **Tags:** #trick #adiabatic #geometric-phase #gauge-choice #eigenstate-basis

## What it does

Fixes the (physically irrelevant) phase of each instantaneous eigenstate such that the diagonal tunneling amplitude $\beta_{\nu,\nu}(s) = \langle\dot\nu(s)|\nu(s)\rangle = 0$ everywhere. This absorbs the geometric (Berry) phase into the eigenstate definition, simplifying all subsequent analysis.

## The trick

Instantaneous eigenstates $|\nu(s)\rangle$ are only defined up to a phase $e^{i\theta_\nu(s)}$. The tunneling amplitude between distinct states:

$$\beta_{\nu,\mu}(s) = \langle\nu(s)|\dot H(s)|\mu(s)\rangle / (E_\nu(s) - E_\mu(s)), \quad \nu \neq \mu$$

is gauge-invariant (the phases cancel). But the diagonal term $\langle\dot\nu|\nu\rangle = i\dot\theta_\nu + \text{(gauge-independent Berry phase)}$ depends on the gauge choice.

**The choice:** Set $\theta_\nu(s)$ so that $\langle\dot\nu(s)|\nu(s)\rangle = 0$ for all $\nu, s$. This is always possible: integrate the geometric phase into the definition of $|\nu(s)\rangle$.

**Consequence:** The eigenstate derivative expands cleanly as:

$$|\dot\nu(s)\rangle = -\sum_{m \neq \nu} |m(s)\rangle\beta_{m,\nu}(s)$$

No diagonal term. The evolution in the instantaneous eigenbasis involves only off-diagonal tunneling amplitudes $\beta_{\nu\mu}$ with $\nu \neq \mu$, which are bounded by $\|\dot H\|/\gamma_{\min}$.

Also: the adiabatic approximation becomes $U(1,0)|G(0)\rangle \approx e^{-iT\int_0^1 E_G(\xi)d\xi}|G(1)\rangle$ with no extra Berry phase factor.

## When to reach for it

- Any path-integral or perturbative analysis of adiabatic evolution — use this gauge from the start to keep expressions clean.
- When computing tunneling amplitudes or proving bounds on eigenstate derivatives.
- Specifically needed for the [[Tunneling Amplitude Derivative Bound via Resolution of Identity]] trick.

## Complexity

Zero cost: it's a choice of basis, not a computation.

## Caveat

The gauge is non-local: $\langle\dot\nu(s)|\nu(s)\rangle = 0$ must hold everywhere, which means you're integrating a differential equation for $\theta_\nu(s)$ along the path. If you want a gauge that is also smooth at level crossings, more care is needed. For non-degenerate paths away from crossings, the gauge is straightforward to implement.

## Related notes
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Tunneling Amplitude Derivative Bound via Resolution of Identity]]
- [[Adiabatic Error as Sum Over Non-Adiabatic Paths]]
