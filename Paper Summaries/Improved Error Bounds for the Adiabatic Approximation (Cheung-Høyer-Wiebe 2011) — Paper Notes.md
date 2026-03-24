> **Source:** Donny Cheung, Peter Høyer, Nathan Wiebe, *Improved Error Bounds for the Adiabatic Approximation*, arXiv:1103.4174, J. Phys. A: Math. Theor. **44**, 415302 (2011)
> **Links:** [arXiv](https://arxiv.org/abs/1103.4174) · [Journal](https://doi.org/10.1088/1751-8113/44/41/415302)
> **Tags:** #adiabatic #error-bounds #adiabatic-theorem #path-integrals #marzlin-sanders

---

## What the paper does

Derives rigorous upper and lower bounds on the error in the adiabatic approximation that are asymptotically tight in the limit of slow evolution (large $T$) for fixed Hamiltonians. The bounds handle the regime where higher-order derivatives of $H(s)$ are large — including Hamiltonians that look like the Marzlin–Sanders counterexample — and resolve the standing question of what condition actually guarantees adiabaticity.

This is a foundational analysis paper. It gives you honest bounds rather than asymptotic estimates: if you need to certify that an adiabatic algorithm runs correctly, this is the tool.

---

## The computational problem

**Setting:** A quantum system on $\mathbb{C}^N$ evolves under a time-dependent Hamiltonian $H(s)$, $s = t/T \in [0,1]$, starting from a non-degenerate instantaneous eigenstate $|G(0)\rangle$.

**The adiabatic approximation:** Claims $U(1,0)|G(0)\rangle \approx e^{-i\int_0^1 E_G(\xi)d\xi \cdot T}|G(1)\rangle$.

**Error:** $\|(I - P_G(1))U(1,0)|G(0)\rangle\|$ — projection of the evolved state onto the complement of $|G(1)\rangle$.

**Goal:** Upper and lower bounds on this error in terms of $\|\dot{H}\|$, $\|\ddot{H}\|$, $\|\dddot{H}\|$, $\gamma_{\min}$, and $T$.

---

## Background: the JRS bound and its problem

The best prior bound was due to Jansen, Ruskai, and Seiler (JRS 2007):

$$\text{error} \leq \frac{m\|\dot{H}(0)\|}{\gamma(0)^2 T} + \frac{m\|\dot{H}(1)\|}{\gamma(1)^2 T} + \frac{1}{T}\int_0^1 \left(\frac{m\|\ddot{H}(\xi)\|}{\gamma(\xi)^2} + \frac{7m\sqrt{m}\|\dot{H}(\xi)\|^2}{\gamma(\xi)^3}\right)d\xi$$

This bound has two problems. First, it depends on $\|\ddot{H}(s)\|$, which diverges for Marzlin–Sanders-type Hamiltonians. Second, JRS noted that their third term is actually $O(1/T^2)$, so their bound is not asymptotically tight — meaning it cannot be inverted to derive a sufficiency condition for the standard $O(1/T)$ estimate.

Cheung–Høyer–Wiebe fix both problems.

---

## Key definitions

**Definition 1.** With $H(s)$ three-times differentiable, define timescales:

$$\Delta_0 := \frac{\|\dot{H}\|}{\min_{\nu,s}[\gamma_{\nu,G}(s)]^2} + \frac{\Delta_1}{\gamma_{\min} T}$$

$$\Delta_1 := \frac{1}{\gamma_{\min}}\left(\frac{\|\dddot{H}\|}{\gamma_{\min}} + \frac{\|\ddot{H}\|^3}{\gamma_{\min}^3} + \frac{\|\dot{H}\|^6}{\gamma_{\min}^6}\right)$$

where $\gamma_{\min}$ is the minimum gap between any two non-degenerate eigenstates, and $\|\dot{H}\|$, $\|\ddot{H}\|$, $\|\dddot{H}\|$ are maxima over $s \in [0,1]$.

---

## Main results

### Theorem 1 (Zeroth-order adiabatic theorem)

If $H$ is three-times differentiable, $\gamma_{\min} > 0$, $|G(s)\rangle$ is non-degenerate, and $\Delta_0 \in o(T)$, then:

$$\|(I - P_G(1))U(1,0)|G(0)\rangle\| = O(\Delta_0/T)$$

This reduces to the standard criterion (Eq. 3) when $H$ doesn't depend on $T$: $\|\dot{H}\|/\gamma_{\min}^2 \in o(T)$.

### Theorem 2 (First-order adiabatic theorem)

If additionally $\Delta_1 \in o(\gamma_{\min} T^2)$, then the error equals:

$$\sum_{\nu \neq G} e^{-i\phi_\nu}|\nu(1)\rangle \left[\frac{\langle\dot{\nu}(s_1)|G(s_1)\rangle e^{-i\int_0^{s_1}\gamma_{G,\nu}(\xi)d\xi \cdot T}}{-i\gamma_{G,\nu}(s_1)T}\right]_{s_1=0}^{1} + O\!\left(\frac{\Delta_1}{\gamma_{\min} T^2}\right)$$

The leading term only depends on properties of $H$ at $s = 0$ and $s = 1$. This is not a surprise: in the large-$T$ limit, rapidly oscillating contributions from interior times cancel by phase — only the "incomplete oscillations" at the endpoints contribute. See [[Adiabatic Approximation Boundary-Term Dominance]] trick card.

### Theorem 4 (The core bound)

The full non-asymptotic bound. For any $T \geq 0$:

$$\left\|(I - P_G(1))U(1,0)|G(0)\rangle - \sum_{\nu \neq G} e^{-i\phi_\nu}|\nu(1)\rangle \frac{\langle\dot{\nu}(s_1)|G(s_1)\rangle e^{-i\int_0^{s_1}\gamma_{G,\nu}(\xi)d\xi \cdot T}}{-i\gamma_{G,\nu}(s_1)T}\bigg|_{s_1=0}^{1}\right\| \leq \frac{R}{T^2}$$

where $R$ is a sum of terms involving $\|\dot{H}\|$, $\|\ddot{H}\|$, $\|\dddot{H}\|$ divided by powers of $\gamma_{\min}$, plus an exponential correction from multi-jump paths.

**This gives both an upper and lower bound** (the leading term $\pm R/T^2$) that converge in the limit of large $T$, making it asymptotically tight when the leading term doesn't vanish.

### Corollary 1

If $\|\ddot{H}\| \in o(\sqrt{T})$, $\|\dddot{H}\| \in o(T)$, and $\|\dot{H}\|$, $\gamma_{\min}^{-1}$ are $O(1)$ in $T$, then:

$$\|(I - P_G(1))U(1,0)|G(0)\rangle\| \in O\!\left(\max_{s=0,1}\frac{\|\dot{H}(s)\|}{\min_\nu[\gamma_{\nu,G}(s)]^2 T}\right)$$

The standard $O(1/T)$ estimate holds even when $\ddot{H}$ and $\dddot{H}$ diverge — just slowly enough.

### Corollary 2

If $\|\ddot{H}\| \in o(T)$ and $\|\dot{H}\|$, $\gamma_{\min}^{-1}$ are $O(1)$, then:

$$\lim_{T\to\infty}\|(I-P_G(1))U(1,0)|G(0)\rangle\| = 0$$

Adiabaticity holds, but the error might not be $O(1/T)$.

---

## The proof technique: path integrals over instantaneous eigenbases

This is the real contribution of the paper. The proof approach is elegant and worth understanding.

### Paths

Discretise time into $L$ steps. At each step, project onto an instantaneous eigenstate of $H$. As $L \to \infty$, the time-evolution operator becomes a sum over paths — sequences of labels $(\nu_0 = G, \nu_1, \ldots, \nu_q)$ with jump times $(s_1, \ldots, s_q)$:

$$U(1,0)|G(0)\rangle = \sum_{q=0}^\infty \int_{J_q} e^{-i\phi(\text{path})} \prod_{\ell} \beta_{\nu_{\ell+1},\nu_\ell}(s_\ell) \cdot |\nu_q(1)\rangle \, d(J_q)$$

where $\beta_{\nu,\mu}(s) = \langle\dot{\nu}(s)|\mu(s)\rangle = \langle\nu(s)|\dot{H}(s)|\mu(s)\rangle / (E_\nu(s) - E_\mu(s))$ is the tunneling amplitude between eigenstates.

The **adiabatic approximation** is the $q = 0$ (zero-jump) term. The **error** is the sum over all non-adiabatic (non-zero-jump, non-returning) paths. See [[Adiabatic Error as Sum Over Non-Adiabatic Paths]] trick card.

### Integration by parts to bound path integrals

In the large-$T$ limit, the phases $e^{-i\phi(\text{path}) \cdot T}$ oscillate rapidly, causing cancellation. IBP makes this quantitative: it trades each power of $T$ in the denominator for a derivative of the tunneling amplitude, extracting the leading-order contribution (the boundary term at $s = 0$ and $s = 1$) and bounding the remainder.

The recursion on the number of jumps (Lemma 2) plus a resolution-of-unity trick to avoid factors of $N$ in the $\beta$-derivatives (Lemma 3) gives the geometric series that bounds all multi-jump contributions. See [[Tunneling Amplitude Derivative Bound via Resolution of Identity]] trick card.

---

## The Marzlin–Sanders counterexample resolved

The counterexample Hamiltonian has $\|\ddot{H}\| \in O(T)$ — exactly the threshold of Corollary 2. Substituting this into Theorem 4, the error bound is $\Theta(1)$, consistent with (and corroborating) the observed failure of adiabaticity. Their criterion $\|\dot{H}\|/\gamma_{\min}^2 \in o(T)$ is satisfied, but $\Delta_0 \not\in o(T)$ because $\Delta_1$ diverges.

The resolution: the traditional criterion (Eq. 3) is necessary but not sufficient when $H$ is allowed to depend on $T$. The correct condition is $\Delta_0 \in o(T)$, which includes a correction from higher derivatives.

**Key point from the paper:** if $\|\ddot{H}\| \in o(T)$ (slightly slower divergence than the counterexample), adiabaticity would hold. The counterexample is marginal — it lives right at the threshold.

---

## Application: Search Hamiltonian

For linear interpolation between $H_0$ and $H_m$ on $N$-item search space, the paper verifies numerically that Theorem 4 bounds are:
- Tighter than JRS in the large-$T$ regime
- Not asymptotically tight when $T = 2n\pi / \int_0^1 \gamma_{1,G}(s)ds$ (integer multiples of the full oscillation) — at these times the $O(1/T)$ leading term vanishes by cancellation of the boundary contributions, and the error drops to $O(1/T^2)$

That last observation (suppression of a specific transition probability to $O(1/T^2)$ at particular times) is flagged as potentially useful for improving adiabatic quantum gates.

---

## Comparison with prior work

| Bound | Asymptotically tight? | Handles $\|\ddot{H}\| = \Theta(T)$? | Works for fixed $H(s)$? |
|---|---|---|---|
| Standard (Eq. 3) | Yes (for fixed $H$) | No | Yes |
| JRS (2007) | No | Yes | Yes |
| **This paper** | **Yes (for fixed $H$)** | **Yes** | **Yes** |

---

## Limits / caveats

- **Non-degenerate assumption throughout.** The analysis assumes $|G(s)\rangle$ is non-degenerate for all $s$. Level crossings break the path-integral IBP argument. Degenerate generalisation left as open problem.
- **Asymptotic tightness is conditional.** Bounds are tight only when the leading $O(1/T)$ term doesn't vanish. For the search Hamiltonian, there are specific times where the leading term cancels (Eq. 71).
- **$T$ large regime.** The exponential correction term in $R$ (involving $e^{\Gamma/\gamma_{\min}T}$) is benign only for large $T$; for small $T$ the JRS bound is better.
- **Infinite-dimensional / unbounded $H$:** not covered.

---

## Reusable ideas

1. **[[Adiabatic Error as Sum Over Non-Adiabatic Paths]]** — the adiabatic approximation error equals the sum over all non-adiabatic paths in the instantaneous eigenbasis. The zero-jump path is the adiabatic evolution itself; all others contribute error. Convergence follows from phase cancellation.

2. **[[Adiabatic Approximation Boundary-Term Dominance]]** — in the large-$T$ limit, interior contributions to adiabatic error cancel by rapid phase oscillation; only boundary terms (at $s=0$ and $s=1$) survive to $O(1/T)$. This is why Theorem 2's leading expression depends only on $H(0)$ and $H(1)$.

3. **[[Tunneling Amplitude Derivative Bound via Resolution of Identity]]** — bounding $\|\partial_s \beta\|$ and $\|\partial_s^2 \beta\|$ without acquiring factors of $N$ uses the resolution of identity to write $|\dot\nu\rangle = -\sum_m |m\rangle\beta_{m,\nu}$, converting eigenstate derivatives into $\beta$'s. This is the key trick for making the bounds dimension-independent.

4. **[[Geometric-Phase-Cancelling Eigenstate Gauge Choice]]** — choosing eigenstate phases such that $\langle\dot\nu(s)|\nu(s)\rangle = 0$ absorbs the geometric phase and dramatically simplifies the path integral.

---

## References within this paper

- **JRS 2007 (Jansen-Ruskai-Seiler):** J. Math. Phys. 48:102111. The bound this paper improves on.
- **Marzlin-Sanders 2004:** PRL 93:160408. The counterexample that motivated this work.
- **Ambainis-Regev 2004:** arXiv:quant-ph/0411152. Discretisation approach and path decomposition used as starting point.
- **Mackenzie-Marcotte-Paquette (MMP) 2006:** PRA 73:042104. Prior path-integral approach (with perturbation theory and convergence issues this paper avoids).
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] — search Hamiltonian used as test case; Eq. 71 optimal schedule connects to their local adiabatic schedule.
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi et al. (2000)]] — original adiabatic computation proposal.
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — uses adiabatic theorem (Theorem 2.1 there) for equivalence proof; the bounds here are sharper than what that paper needed.
- **Rezakhani-Pimachev-Lidar 2010:** arXiv:1008.0863. Also studies short-time regime and higher-order approximations; the zero-radius-of-convergence observation is shared.
- **Reichardt 2004 (STOC):** Also has upper bounds for adiabatic approximation via IBP; JRS improved on Reichardt.

---

## Cross-links

### Paper notes
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]]
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]

### Trick cards
- [[Adiabatic Error as Sum Over Non-Adiabatic Paths]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Tunneling Amplitude Derivative Bound via Resolution of Identity]]
- [[Geometric-Phase-Cancelling Eigenstate Gauge Choice]]
- [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]]
- [[Gap-Adapted Adiabatic Scheduling]]
