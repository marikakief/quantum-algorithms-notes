> **Source:** Nathan Wiebe, Dominic Berry, Peter Høyer, and Barry C. Sanders, *Higher Order Decompositions of Ordered Operator Exponentials*, arXiv:0812.0562, J. Phys. A: Math. Theor. **43**, 065203 (2010)
> **Links:** [arXiv](https://arxiv.org/abs/0812.0562)
> **Tags:** #hamiltonian-simulation #product-formulas #suzuki #time-dependent #ordered-exponential #trotter

---

## The computational problem

Approximate the ordered operator exponential

$$
U(\lambda, \mu) = \mathcal{T}\exp\!\int_\mu^\lambda H(u)\,du, \qquad H(u) = \sum_{j=1}^m H_j(u),
$$

where each $e^{H_j(u)\delta}$ can be computed efficiently, by a product of ordinary (unordered) operator exponentials. The complexity measure is the number $N$ of simple exponentials in the approximating product.

This is the time-dependent Hamiltonian simulation problem in the "freeze-and-exponentiate at midpoint" product-formula style — distinct from the [[Generalised Trotter for Time-Dependent Hamiltonians|integrating version]] of Poulin et al. and from Dyson-series approaches. The paper's goal is rigorous error bounds that don't rely on analyticity of $H(u)$.

---

## What the paper does

Shows that [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki's recursive product formulas]] extend cleanly to ordered exponentials, provided $H(u)$ is sufficiently smooth, with explicit bounds on how smoothness limits the achievable order. The key result: for $\Lambda$–$2k$-smooth $H$, the $k$-th order Suzuki formula $U_k$ achieves error $O(\Delta\lambda^{2k+1})$ on a step of size $\Delta\lambda$, and the total number of exponentials to reach accuracy $\varepsilon$ over interval $[\mu, \mu+\Delta\lambda]$ is bounded in terms of $\Lambda$, $k$, $m$, and $\varepsilon$.

The paper is a careful foundation paper. Suzuki (1991) gave the recursion; Berry et al. (2005) exploited it for time-independent simulation. What's new here is proving it works for ordered exponentials without assuming $H(u)$ is analytic — and making the dependence on smoothness explicit so you know exactly when and why the method fails.

There's also a clean corollary: if $H$ is $\infty$-smooth, optimizing the Suzuki order gives near-linear scaling in $\Delta\lambda$, matching what [[Suzuki Order as a Tunable Knob for Time Scaling|Berry et al. (2005)]] got for the time-independent case.

---

## The algorithm / construction

### Base integrator $U_1$

For a step of size $\Delta\lambda$ on $[\mu, \mu+\Delta\lambda]$, the symmetric second-order Suzuki formula evaluates all terms at the midpoint $\mu + \Delta\lambda/2$:

$$
U_1(\mu+\Delta\lambda, \mu) = \left(\prod_{j=1}^m e^{H_j(\mu+\Delta\lambda/2)\,\Delta\lambda/2}\right)\!\left(\prod_{j=m}^1 e^{H_j(\mu+\Delta\lambda/2)\,\Delta\lambda/2}\right).
$$

This has error $O(\Delta\lambda^3)$ — the time-dependence contributes at the same order as non-commutativity, so neither one makes things worse.

### Suzuki recursion ($U_p \to U_{p+1}$)

Given $U_p$ (symmetric, error $O(\Delta\lambda^{2p+1})$), define

$$
U_{p+1}(\mu+\Delta\lambda, \mu) = U_p(\Delta\lambda_{5}) \cdot U_p(\Delta\lambda_{4}) \cdot U_p(\Delta\lambda_{3}) \cdot U_p(\Delta\lambda_{2}) \cdot U_p(\Delta\lambda_{1}),
$$

where the five substep intervals have sizes

$$
s_p\Delta\lambda,\;\; s_p\Delta\lambda,\;\; (1-4s_p)\Delta\lambda,\;\; s_p\Delta\lambda,\;\; s_p\Delta\lambda, \quad s_p = \frac{1}{4 - 4^{1/(2p+1)}}.
$$

Applying this recursion $k-1$ times starting from $U_1$ produces $U_k$, a product of at most $2m \cdot 5^{k-1}$ exponentials.

### Long intervals: splitting

If $\Delta\lambda$ is too large for the single-step bound to hold, split into $r$ equal subintervals and apply $U_k$ on each. The total number of exponentials is $N \leq r \cdot (2m \cdot 5^{k-1})$.

### Complexity bound (Theorem 1)

If $\{H_j\}$ is $\Lambda$–$2k$-smooth on $[\mu, \mu+\Delta\lambda]$ and $\varepsilon \leq (9/10)(5/3)^k \Lambda\Delta\lambda$, then

$$
N \leq 2m\cdot 5^{k-1} \cdot \left\lceil 5^{k}\Lambda\Delta\lambda\left(\frac{(5/3)^k\Lambda\Delta\lambda}{\varepsilon}\right)^{1/(2k)}\right\rceil.
$$

---

## Key results

**Theorem 3 (single-step error):** Under $\Lambda$–$2k$-smoothness and $2\sqrt{2(5^k-1)}\,Q_k\Lambda\Delta\lambda \leq 1/2$,

$$
\|U(\mu+\Delta\lambda,\mu) - U_k(\mu+\Delta\lambda,\mu)\| \leq 2\bigl[3(5^k-1)\,Q_k\Lambda\Delta\lambda\bigr]^{2k+1},
$$

where $Q_k$ is a constant satisfying $\frac{3}{2^{1/3^k}} \leq Q_k \leq \frac{2^k}{3^k}$ (defined by the Suzuki recursion structure; decreases with $k$).

**Corollary 1:** If $\{H_j\}$ is $2k$-smooth, the $k$-th order formula has error $O(\Delta\lambda^{2k+1})$. The order $2k$ is exactly what you pay for and exactly what you get — but only if you have $2k$ derivatives.

**Near-linear scaling (∞-smooth case):** For $H$ that is $\Lambda$–$\infty$-smooth, choose

$$
k_0 = \left\lceil \sqrt{\tfrac{1}{2}\log_{25/3}\!\left(\Lambda\Delta\lambda/\varepsilon\right)}\,\right\rceil.
$$

Then $N$ grows sub-polynomially in $\Delta\lambda$ — essentially linear, matching the Berry 2005 time-independent result.

---

## Comparison with prior work

| Paper | Setting | Error bounds | Smoothness requirement |
|---|---|---|---|
| Suzuki (1991) | Time-independent $H$ | Formal order, no explicit constants | Implicitly analytic |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes\|Berry et al. (2005)]] | Time-independent sparse | Explicit, near-optimal | Not applicable |
| This paper | Time-dependent $H(u)$ | Explicit, rigorous, non-analytic $H$ | $\Lambda$–$2k$-smooth (Definition 2) |
| [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes\|Poulin et al. (2011)]] | Time-dependent | Derivative-free (different approach) | None (but weaker scaling) |

The main improvement over prior product-formula work is rigour: the proof goes through without assuming $H(u)$ is analytic, and the smoothness condition is stated explicitly rather than assumed away.

---

## Limits / caveats

- The smoothness condition is necessary, not just sufficient. If $H$ has only $P$ bounded derivatives, you cannot achieve order beyond $P$ regardless of the Suzuki recursion depth. Arbitrary-order scaling fails.
- The $5^k$ constant blowup in the number of exponentials is real, not an artifact. For high $k$, this can dominate.
- The bound assumes $\max_{x>y}\|U(x,y)\| \leq 1$ — i.e., the propagator is norm-non-increasing. This holds for unitary evolution (anti-Hermitian $H$) but needs checking in other settings.
- This paper predates Dyson-series / LCU methods ([[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry 2018]]) and interaction-picture simulation ([[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe 2018]]), which get better parameter dependence at the cost of more machinery. For smooth $H$, this paper's near-linear product-formula approach remains competitive.

---

## Reusable ideas

1. [[Suzuki Recursion for Ordered Exponentials]] — the key extension: the five-term Suzuki recursion, evaluated at appropriate subintervals of $[μ, μ+Δλ]$, works for time-dependent $H$ with the same order-improvement as the time-independent case.
2. [[Smoothness-Order Saturation for Product Formulas]] — Suzuki order $2k$ requires $H$ to have $2k$ bounded derivatives; the order you can extract is capped by the smoothness class, and the bounds degrade gracefully rather than blowing up.
3. [[Λ-Smoothness Parametrization]] — packaging all derivative norms into a single parameter $\Lambda$ via $\sum_j \|H_j^{(p)}(u)\| \leq \Lambda^{p+1}$; makes complexity bounds clean and portable to other contexts.

---

## References within this paper

- Suzuki (1991), Phys. Lett. A **146**, 319 — the recursive integrator construction this paper extends. Not yet in vault.
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — time-independent version; this paper extends their near-linear scaling result to ordered exponentials.
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — first product-formula quantum simulation; the context for why this matters.
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2019)]] — later sharpens Trotter error analysis using nested commutators (post-dates this paper).
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer & Berry (2018)]] — Marika's paper; extends to Dyson/LCU methods for time-dependent H. Explicitly builds on the problem formulation here.

---

## Cross-links

### Paper notes
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]] — companion algorithmic paper; builds the quantum algorithm on the integrator bounds proved here
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — uses the Suzuki recursion and Yoshida time-reversibility from this paper as the structural foundation for randomized order-doubling
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]]
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]]

### Trick cards
- [[Suzuki Recursion for Ordered Exponentials]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Λ-Smoothness Parametrization]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — uses the same Yoshida construction to find dramatically improved 8th-order product formulas via numerical optimization with over-parameterized ansätze
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — multi-product formula randomization; the MPF error analysis directly uses the Suzuki product-formula bounds established here
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry-protected simulation; applies the product-formula framework developed here with symmetry kicks inserted between steps
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — empirical study showing the practical Trotter error for chemistry is far below the norm bounds; directly motivated by the bounds established in this paper
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — fault-tolerant Trotter simulation; uses high-order product formulas from this lineage for condensed-phase Hamiltonians
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — injected BCH correctors that improve effective order; builds directly on the error expansion framework for product formulas developed here
