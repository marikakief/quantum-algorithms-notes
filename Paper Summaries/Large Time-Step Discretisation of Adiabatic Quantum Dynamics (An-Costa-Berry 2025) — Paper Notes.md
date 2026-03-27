> **Source:** Dong An, Pedro C. S. Costa, and Dominic W. Berry, *Large time-step discretisation of adiabatic quantum dynamics*, arXiv:2509.00171 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2509.00171) · [PDF](https://arxiv.org/pdf/2509.00171)
> **Tags:** #adiabatic #product-formulas #trotter #discrete-adiabatic #grover #QAOA #eigenstate-preparation #spectral-gap

---

## The computational problem

**Eigenstate preparation via digital adiabatic quantum computing.** Given Hamiltonians $H_0$ (easy) and $H_1$ (target), with $H(s) = (1-f(s))H_0 + f(s)H_1$, prepare the eigenstate of $H_1$ connected to a known eigenstate of $H_0$ by adiabatic evolution along the eigenpath of $H(s)$.

The question: how large can the time step $h$ be when simulating the adiabatic dynamics digitally via [[Product Formulas]]s or exponential integrators, and what is the overall query complexity?

---

## What the paper does

Shows that time step sizes for digitally simulating adiabatic dynamics can be **independent of the tolerated error $\varepsilon$ and evolution time $T$** — far larger than standard numerical analysis predicts. For first-order methods on bounded Hamiltonians, $h = O(1)$ suffices. This is a genuinely surprising result: you don't need to shrink the time step to improve accuracy. You improve accuracy by running longer (larger $T$), not by refining the discretisation.

Under the boundary cancellation condition, the paper provides strong evidence (numerical + conditional on a conjecture about high-order discrete adiabatic theorems) that even first-order Trotter with uniform $O(1)$ step size achieves **super-polynomial convergence** $O(1/T^k)$ for any $k$.

As an application, the Trotterised adiabatic approach to unstructured search matches the [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] lower bound $O(\sqrt{N})$, doesn't need to know the number of marked states, and produces QAOA angles that are asymptotically as good as optimised [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|QAOA]].

My assessment: this is an important paper. The conceptual shift — viewing numerical integrators as discrete adiabatic walk operators rather than as approximations to continuous evolution — is the real contribution. It unifies and strengthens previous results, explains phenomena that weren't understood (Trotter working despite gapless Hamiltonians), and has concrete algorithmic consequences. The main caveat is that the exponential convergence results rest on Conjecture 10 (the high-order [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] from Dranov-Kellendonk-Seiler 1998, whose proof has a gap they identify but can't fix).

---

## The algorithm / construction

### Key conceptual shift

Standard analysis bounds the total error as:

$$\text{total error} \leq \underbrace{\text{continuous adiabatic error}}_{O(\alpha^2/(\Delta_*^3 T))} + \underbrace{\text{time discretisation error}}_{\text{depends on } h, T, \varepsilon}$$

This paper skips the decomposition entirely. Instead, treat each numerical propagator $U_{\text{num}}((j+1)h, jh)$ as a walk operator in its own right and apply the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] directly to the sequence $\{U_{\text{num}}\}$. The error is then bounded by a single term — the discrete adiabatic error — which depends on: (1) how slowly the walk operators change (controlled by $f'(s)/T$), and (2) the spectral gap of the walk operators.

The second observation: the Hamiltonian $H(t/T)$ already varies on the slow timescale $s = t/T$, which controls the rate of change of the walk operators. So the time step $h$ doesn't need to be small to ensure slow variation — the scheduling function handles that.

### Numerical methods considered

**First-order exponential integrator:**
$$U_{\exp}(t+h, t) = e^{-ihH(t/T)}$$

**Simplified $p$-th order [[Product Formulas]]:**
$$U_{\text{spf},p}(t+h, t) = \prod_{k=0}^{K_p} e^{-i\beta_{p,k} h f(t/T) H_1} \, e^{-i\alpha_{p,k} h (1-f(t/T)) H_0}$$

The "simplified" version evaluates all scheduling functions at the same time $t/T$ rather than at staggered points within the step. This matters because it makes the walk operator a function of a single parameter $s = t/T$, fitting cleanly into the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] framework.

### Gap matching for [[Product Formulas]]s

For the exponential integrator with $h \leq 1/\alpha$ (where $\alpha = \|H_0\| + \|H_1\|$), the gap of $e^{-ihH(s)}$ equals the gap of $hH(s)$ — inherited directly from the Hamiltonian.

For [[Product Formulas]]s, it's harder. The eigenvalues of $e^{-ihf(s)H_1}e^{-ih(1-f(s))H_0}$ are not trivially related to those of $H(s)$ at large $h$. The paper's strategy: reduce $h$ until the [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Trotter error bound]] guarantees the product walk operator is close enough to $e^{-ihH(s)}$ to inherit its gap. The step size needed is:

$$h = O\!\left(\min\!\left\{\frac{1}{\alpha}, \frac{\Delta_*^{1/p}}{\tilde{\alpha}_p^{1/p}}\right\}\right)$$

where $\tilde{\alpha}_p = \sum_{\gamma_0,\ldots,\gamma_p \in \{0,1\}} \|[H_{\gamma_p}, \ldots, [H_{\gamma_1}, H_{\gamma_0}]]\|$ is the nested commutator norm from [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]].

The critical point: this $h$ depends only on the Hamiltonians' norms, commutators, and gap — **not on $\varepsilon$ or $T$**.

A nice structural observation: first- and second-order simplified product formulas share the same eigenvalues (Lemma 6), because $U_{\text{spf},2} = V U_{\text{spf},1} V^\dagger$ for $V = e^{-ih(1-f(s))H_0/2}$.

### Boundary cancellation and super-polynomial convergence

When the scheduling function $f(s)$ satisfies $f^{(k)}(0) = f^{(k)}(1) = 0$ for all $k \geq 1$, the walk operators inherit the boundary cancellation condition. Under Conjecture 10 (the high-order [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]]):

- Step size $h = O(1)$ suffices
- Total steps $T_d = O(1/\varepsilon^{1/k})$ for any $k$ — super-polynomial convergence
- This holds even for first-order methods

The conjecture is due to Dranov-Kellendonk-Seiler (1998). The authors find a **missing step in the original proof** (detailed in Appendix F.2) but verify the conclusion numerically.

### Gapless Hamiltonians: Trotter opens gaps

Section V is the most provocative part. When $H(s)$ is gapless (or has exponentially small gap), continuous AQC fails. But the first-order Trotter operator $e^{-if(s)H_1}e^{-i(1-f(s))H_0}$ with $h = 1$ can have a finite gap even when $H(s)$ doesn't.

The paper demonstrates this with a 4-dimensional toy model. Two examples:
1. $H(s)$ gapped but $W(s)$ gapless (gap of walk operator closes even though Hamiltonian gap doesn't)
2. $H(s)$ gapless but $W(s)$ gapped (walk operator has finite gap even though Hamiltonian is gapless)

In case 2, discrete adiabatic evolution with $h = 1$ succeeds where continuous AQC provably fails. Reducing the step size actually makes things worse — it forces the numerical solution to approximate the failing continuous dynamics.

This is a tantalising observation. If there exist problems of practical interest where this phenomenon occurs, discrete AQC could offer exponential speedups over continuous AQC. The paper leaves finding such applications as an open question.

---

## Key results

**Theorem (Corollary 4 — exponential integrator):** For $h = 1/\alpha$ and $T = O(\alpha^2/(\Delta_*^3 \varepsilon))$, the total number of steps is:

$$T_d = O\!\left(\frac{\alpha^3}{\Delta_*^3 \varepsilon}\right)$$

Compare standard analysis: $T_d = O(\alpha^3 \Delta_*^{-3} \varepsilon^{-2})$. The improvement is a full factor of $1/\varepsilon$.

**Theorem (Corollary 8 — first/second-order [[Product Formulas]]):** Same $T$, with step size:

$$h = \min\!\left\{\frac{1}{\alpha},\; \sqrt{\frac{95}{2}} \sqrt{\frac{\Delta_*}{2\|[H_1,[H_1,H_0]]\| + \|[H_0,[H_0,H_1]]\|}}\right\}$$

Total steps:

$$T_d = O\!\left(\frac{\alpha^3}{\Delta_*^3 \varepsilon} \max\!\left\{1, \sqrt{\frac{2\|[H_1,[H_1,H_0]]\| + \|[H_0,[H_0,H_1]]\|}{\Delta_* \alpha^2}}\right\}\right)$$

**Theorem (Corollary 9 — $p$-th order simplified [[Product Formulas]]):** For any $p \geq 1$:

$$T_d = O\!\left(\frac{\alpha^3}{\Delta_*^3 \varepsilon} \max\!\left\{1, \frac{\tilde{\alpha}_p^{1/p}}{\Delta_*^{1/p} \alpha}\right\}\right)$$

**Theorem 11 / 12 (boundary cancellation, conditional on Conjecture 10):** With $\|H_0\|, \|H_1\| \leq 1$, constant gap, and $f^{(k)}(0) = f^{(k)}(1) = 0$:

$$T_d = O(1/\varepsilon^{1/k}) \quad \text{for any } k \geq 1$$

**Theorem 15 (Grover search with $p$-schedule):** For the scheduling function $f'(s) = d_{N,p} \Delta_H(s; N, 1, f(s))^p$:
- $p = 1$: total steps $O\!\left(\sqrt{N/M} \cdot \log(N)/\varepsilon\right)$ — near-optimal in $N$ and $M$
- $1 < p < 2$: total steps $O\!\left(\sqrt{N}/M^{1-p/2} \cdot 1/\varepsilon\right)$ — optimal in $N$

**Theorem 16 (Grover search with boundary-cancellation schedule):** Using $f(s)$ built from two copies of $g(s) = c_e^{-1}\int_0^s e^{-1/(s'(1-s'))} ds'$:
- For constant error: $\tilde{O}(\sqrt{N/M})$ steps — Grover scaling
- For fixed $N, M$: $O(1/\varepsilon^{o(1)})$ steps — super-polynomial precision convergence (conditional on Conjecture 10)
- Schedule is independent of both $M$ and $N$

---

## Comparison with prior work

| Method | Time step $h$ | Total steps | Reference |
|---|---|---|---|
| Standard 1st-order exp. integrator | $O(\varepsilon/\alpha)$ | $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-2})$ | Textbook |
| Standard $p$-th order PF | $O(\varepsilon^{1/p} \alpha^{-1-1/p} T^{-1/p})$ | $O(\alpha^{3+3/p} \Delta_*^{-3-3/p} \varepsilon^{-1-2/p})$ | [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes\|Wiebe-Berry-Høyer-Sanders (2010)]] |
| Yi (2021) | $O(1)$ (1st-order Trotter only) | $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-1})$ | Phys. Rev. A 104, 052603 |
| Kovalsky et al. (2023) | $O(1)$ (1st-order Trotter only) | $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-1})$ | PRL 131, 060602 |
| **This paper** (exp. integrator) | $1/\alpha$ | $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-1})$ | Corollary 4 |
| **This paper** ($p$-th order PF) | $O(\min\{1/\alpha, \Delta_*^{1/p}/\tilde{\alpha}_p^{1/p}\})$ | $O(\alpha^3 \Delta_*^{-3} \varepsilon^{-1} \cdot \max\{1, \tilde{\alpha}_p^{1/p}/(\Delta_*^{1/p}\alpha)\})$ | Corollary 9 |
| **This paper** + BCC | $O(1)$ | $O(1/\varepsilon^{o(1)})$ (conditional) | Theorems 11, 12 |

Key improvements over Yi (2021) and Kovalsky et al. (2023):
1. Works for any-order [[Product Formulas]]s, not just first-order Trotter
2. Under boundary cancellation, achieves exponential convergence (they only show linear)
3. Reveals the discrete adiabatic perspective — Trotter can succeed for gapless $H(s)$

---

## Limits / caveats

- **The high-order results are conditional.** Theorems 11, 12, and 16 depend on Conjecture 10 — the high-order discrete adiabatic theorem from Dranov-Kellendonk-Seiler (1998). The authors found a gap in the original proof and couldn't fix it. Numerical evidence supports the conjecture, but it remains unproven.

- **The gap of product walk operators is not well-understood at large $h$.** For the exponential integrator, the gap relation is clean ($h \leq 1/\alpha$ guarantees it). For Trotter at large step sizes, the walk operator gap can differ wildly from the Hamiltonian gap — the paper shows examples in both directions. The "Trotter opens gaps" phenomenon is demonstrated on a 4D toy model only; no natural applications are known yet.

- **Simplified [[Product Formulas]]s only.** The higher-order results (Corollary 9) use simplified formulas where all scheduling evaluations share the same time point. The full higher-order Trotter-Suzuki formula with staggered evaluations isn't directly covered — that would require a discrete adiabatic theorem for non-autonomous sequences, which doesn't exist.

- **The $\Delta_*^{-3}$ gap dependence.** All results (without gap-adapted scheduling) have cubic gap dependence, inherited from the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]]. This is improvable to linear for specific problems using [[Gap-Adapted Adiabatic Scheduling|gap-adapted schedules]] (Section VI demonstrates this for Grover), but the general framework doesn't achieve it.

- **Implementation overhead.** The exponential integrator requires a block encoding of $H(s)$ and QSVT to implement $e^{-ihH(s)}$, adding $O(\log(1/\varepsilon'))$ overhead per step. The [[Product Formulas]] approach doesn't need this — just exponentials of $H_0$ and $H_1$ separately — which is one of its practical advantages.

---

## Reusable ideas

1. [[Discrete Adiabatic View of Numerical Integrators]] — treat time-discretisation operators as walk operators in a discrete adiabatic evolution, bounding error through the discrete adiabatic theorem rather than separating continuous adiabatic error from discretisation error.

2. [[Error-Independent Time Step for Adiabatic Simulation]] — for digitally simulated AQC, the time step can be chosen independent of $\varepsilon$ and $T$; only the Hamiltonian norms, commutators, and gap matter.

3. [[Trotter Gap Independence from Hamiltonian Gap]] — at large step sizes, product-formula walk operators can have spectral gaps unrelated to the Hamiltonian's gap — including opening gaps where the Hamiltonian is gapless.

4. [[Boundary-Cancellation Schedule for Adiabatic Grover Search]] — a smooth scheduling function $f(s) = \frac{1}{2}g(2s)$ (for $s \leq 1/2$) using $g(s) = c_e^{-1}\int_0^s e^{-1/(s'(1-s'))} ds'$, which simultaneously satisfies the boundary cancellation condition and slows down at the gap minimum, achieving $\tilde{O}(\sqrt{N/M})$ with $O(1/\varepsilon^{o(1)})$ precision scaling.

---

## References within this paper

- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa, An, Sanders, Su, Babbush, Berry (2022)]]: Source of the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] with explicit gap dependence — the main technical tool of this paper.
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe, Zhu (2021)]]: Nested commutator Trotter error bounds — used for gap matching in higher-order [[Product Formulas]]s.
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland and Cerf (2002)]]: Local adiabatic scheduling for Grover search ($p = 2$). This paper generalises to $1 \leq p < 2$.
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An and Lin (2019)]]: Scheduling function for QLSP via AQC; the "glue function" scheduling (Eq. 82) originates here.
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi, Goldstone, Gutmann (2014)]]: QAOA formulation. This paper shows discrete AQC produces QAOA angles matching optimal query complexity for Grover search.
- Dranov, Kellendonk, Seiler (1998), J. Math. Phys. 39, 1340: Original discrete adiabatic theorem. This paper finds a gap in the high-order proof.
- Yi (2021), Phys. Rev. A 104, 052603: Showed robustness of Trotterised AQC via effective Hamiltonian approach. This paper extends to all orders and boundary cancellation.
- Kovalsky et al. (2023), PRL 131, 060602: Related first-order-Trotter-specific result. This paper is strictly more general.
- Jansen, Ruskai, Seiler (2007): Continuous adiabatic theorem with explicit gap dependence.
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer, Sanders (2010)]]: Higher-order [[Product Formulas]]s for time-dependent Hamiltonians — the standard analysis this paper improves upon.
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry (2025)]]: Improved product formulas — the simplified versions used here.
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer, Berry (2019)]]: Dyson series for time-dependent simulation — an alternative approach this paper implicitly competes with.
- Dalzell, Yoder, Chuang (2017): Fixed-point adiabatic search with boundary cancellation — the paper builds on and improves their Grover search results.

---

## Cross-links

### Paper notes
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — predecessor; source of the discrete adiabatic theorem used here
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — numerical follow-up to the QLSP solver; same discrete adiabatic framework
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — commutator-based Trotter bounds used for gap matching
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]] — local adiabatic scheduling, which this paper discretises
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — gap-adapted scheduling and AQC-QAOA connection; "glue function" scheduling used here
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — QAOA formulation; shown to be asymptotically matched by discrete AQC for Grover
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the search lower bound this paper matches
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — standard higher-order [[Product Formulas]] analysis that is improved here
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — the simplified [[Product Formulas]]s used in this analysis
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]] — coherent adiabatic error cancellation; different approach to the boundary-term problem this paper addresses via boundary cancellation
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — AQC-circuit equivalence
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the HHL starting point for the broader QLSP lineage that this Costa-An-Berry thread ultimately improves
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — early ODE simulation via history-state linear systems; part of the same Berry line of algorithmic simulation results
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — improved linear ODE solver in the same lineage of precision-sensitive simulation algorithms
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — time-dependent ODE solver from overlapping authors; another branch of the same Berry/Costa simulation program
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — nonlinear ODE extension from the same Costa/Berry lineage; shares the goal of pushing simulation beyond the original linear-system setting
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — Dyson-series time-dependent simulation; a parallel Berry/Kieferová branch that this paper cites as an alternative route to controlling time dependence

### Trick cards
- [[Discrete Adiabatic Theorem for Quantum Walks]] — the core technical tool
- [[Discrete Adiabatic View of Numerical Integrators]] — main conceptual contribution (new)
- [[Error-Independent Time Step for Adiabatic Simulation]] — key practical result (new)
- [[Trotter Gap Independence from Hamiltonian Gap]] — Trotter opening/closing gaps (new)
- [[Boundary-Cancellation Schedule for Adiabatic Grover Search]] — application to search (new)
- [[Gap-Adapted Adiabatic Scheduling]] — scheduling technique used and extended here
- [[AQC-to-QAOA Parameter Transfer]] — the QAOA connection generalised here
- [[Cross-Order Product Formula Threshold]] — relevant for understanding when higher-order helps
- [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]] — alternative approach to adiabatic error reduction
- [[Antisymmetric Path Design for Adiabatic Error Cancellation]] — alternative path design approach
