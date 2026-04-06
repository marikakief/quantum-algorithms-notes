---
aliases:
  - Product Formula (Trotter-Suzuki)
  - Trotter-Suzuki
  - Trotterization
---

**Concept hub** — 86+ wikilink refs across the vault.

---

## The problem

We want to implement $e^{-iHt}$ on a quantum computer, where $H = \sum_{j=1}^{\Gamma} H_j$ and each $e^{-iH_j t}$ is easy to implement (e.g., each $H_j$ acts on a few qubits). The catch: $e^{-i(A+B)t} \neq e^{-iAt}e^{-iBt}$ unless $[A,B]=0$. Product formulas approximate the joint evolution by composing the individual ones, accepting a controlled error from non-commutativity.

---

## First order: Lie–Trotter

The simplest decomposition:

$$
S_1(t) = \prod_{j=1}^{\Gamma} e^{-iH_j t}.
$$

Split time into $r$ steps:

$$
e^{-iHt} \approx \left(\prod_{j=1}^{\Gamma} e^{-iH_j t/r}\right)^r.
$$

Error per step is $O(t^2/r^2)$ from the leading commutator $[H_j, H_k]$, so total error is $O(t^2/r)$. To reach accuracy $\varepsilon$, you need $r = O(t^2/\varepsilon)$ steps — each consisting of $\Gamma$ exponentials. This is Lloyd's original proposal ([[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd 1996]]).

---

## Second order: symmetric Trotter

Symmetrise by running the product forwards then backwards:

$$
S_2(t) = \prod_{j=1}^{\Gamma} e^{-iH_j t/2} \prod_{j=\Gamma}^{1} e^{-iH_j t/2}.
$$

The palindromic structure kills all odd-order error terms. Error per step becomes $O(t^3/r^3)$, so total error $O(t^3/r^2)$ and you need $r = O(t^{3/2}/\varepsilon^{1/2})$ steps. This is usually the practical starting point.

---

## Higher orders: Suzuki's recursion

Suzuki (1991) gave a recursive construction that boosts the order by 2 at each level. Given a symmetric $2k$-th order formula $S_{2k}$, define:

$$
S_{2k+2}(t) = S_{2k}(s_k t)^2 \cdot S_{2k}((1-4s_k)t) \cdot S_{2k}(s_k t)^2,
$$

where $s_k = (4 - 4^{1/(2k+1)})^{-1}$.

This is a five-fold composition with carefully chosen step sizes that cancel the leading error term. The coefficients $s_k$ and $1-4s_k$ are chosen so the $(2k+1)$-th order BCH terms vanish — a direct application of [[Order-Condition Cancellation in Product Formulas|order-condition cancellation]].

**Cost:** each recursion level multiplies the number of exponentials by 5. A $2k$-th order formula uses $2\Gamma \cdot 5^{k-1}$ exponentials per step. The exponential growth in $k$ is real but manageable: for most applications, $k = 1$ (second order) or $k = 2$ (fourth order) is the sweet spot.

**Optimal order selection:** for a time-independent $H$ with $\Gamma$ bounded-norm terms, the total gate count to accuracy $\varepsilon$ with a $(2k)$-th order formula is

$$
N = O\!\left(5^k \Gamma \left(\Gamma \|H\| t\right)^{1+1/(2k)} \varepsilon^{-1/(2k)}\right).
$$

Optimising over $k$ gives near-linear scaling in $t$: $N = (\Gamma \|H\| t)^{1+o(1)}$, with the $o(1)$ from choosing $k \sim \sqrt{\log(\|H\|t/\varepsilon)}$. This was first shown rigorously by [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry et al. (2005)]] and extended to time-dependent Hamiltonians by [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe et al. (2010)]].

---

## Commutator-scaling bounds (Childs–Su–Tran–Wiebe–Zhu 2019)

The results above use crude norm bounds ($\|H_j\|$) to estimate error. The key insight of [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs et al. (2019)]] is that product-formula error is actually governed by *nested commutators*, not norms:

For a $p$-th order formula, define the commutator sum

$$
\tilde{\alpha}_{\mathrm{comm}} = \sum_{\gamma_1,\ldots,\gamma_{p+1}=1}^{\Gamma} \bigl\| [H_{\gamma_{p+1}}, [H_{\gamma_p}, \cdots, [H_{\gamma_2}, H_{\gamma_1}]\cdots]] \bigr\|.
$$

Then:

$$
\|S_p(t) - e^{-iHt}\| = O\!\left(\tilde{\alpha}_{\mathrm{comm}} \cdot t^{p+1}\right).
$$

**Why this matters:** for geometrically local Hamiltonians (spin chains, lattice models), most commutators $[H_j, H_k]$ vanish because distant terms don't overlap. The commutator sum $\tilde{\alpha}_{\mathrm{comm}}$ then scales with the system's *geometry* rather than the total norm. This explains a long-standing puzzle: product formulas work far better in practice than naive norm bounds predict.

For example, [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. (2015)]] observed empirically that norm bounds overestimate Trotter cost by factors up to $10^{16}$ for chemistry Hamiltonians. The commutator-scaling framework provides the theoretical explanation.

These bounds are tight for first and second order, and near-tight numerically for higher orders.

---

## Fermionic refinement (Su–Huang–Campbell 2021)

For electronic structure Hamiltonians, [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes|Su, Huang & Campbell (2021)]] go further: they introduce a *fermionic seminorm* that restricts the error analysis to the physical $\eta$-electron subspace. Since most of Hilbert space is unphysical (wrong particle number), the error on the physical subspace can be much smaller than the operator-norm bound. Their bounds are nearly tight for plane-wave electronic structure and Fermi-Hubbard.

---

## The time-dependent case

When $H = H(t)$ varies with time, the target is the time-ordered exponential $\mathcal{T}\exp(-i\int_0^T H(t)\,dt)$. Product formulas still work — evaluate each term at the midpoint of the time step — but the achievable order is capped by the smoothness of $H(t)$.

Specifically ([[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe et al. 2010]]): if $H(t)$ has $2k$ bounded derivatives (is $\Lambda$–$2k$-smooth), the $k$-th order Suzuki formula achieves error $O(\Delta t^{2k+1})$ per step. If $H$ is $\infty$-smooth, optimising $k$ recovers near-linear scaling just as in the time-independent case.

---

## Randomised product formulas

Deterministic formulas must have all error terms cancel algebraically. Randomisation provides an alternative: let error terms cancel *in expectation*.

| Method | Idea | Error scaling |
|---|---|---|
| [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes\|qDRIFT (Campbell 2019)]] | Randomly sample one $H_j$ per step, weighted by $\|H_j\|$ | $O(\lambda^2 t^2/r)$ where $\lambda = \sum_j \|H_j\|$ |
| [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes\|Childs, Ostrander & Su 2019]] | Randomly permute the ordering within each Trotter step | Doubles effective order (2nd → 4th in expectation) |
| [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes\|Cho, Berry & Hsieh 2022]] | Randomised Suzuki-like correction | Doubles order of any deterministic formula |

qDRIFT is notable because its cost depends on $\lambda = \sum_j \|H_j\|$ (the 1-norm) rather than $\Gamma$ (the number of terms). For Hamiltonians with many small terms, this can be much better than deterministic Trotterization.

---

## Multi-product formulas

Rather than a single product, take a *linear combination* of products at different step sizes:

$$
e^{-iHt} \approx \sum_k c_k \, S(t/r_k)^{r_k}.
$$

The coefficients $c_k$ are chosen to cancel additional error orders. This isn't unitary, so it requires LCU (oblivious amplitude amplification) to implement.

[[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann, Steudtner, Kueng, Kieferová & Eisert (2022)]] randomise the multi-product to control the 1-norm of the coefficients, keeping the success probability reasonable.

---

## Corrected product formulas

[[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab et al. (2024)]] inject BCH corrector terms — short additional circuits that cancel the leading commutator error of a base formula. For "perturbed" Hamiltonians ($H = H_0 + \alpha V$ with $\alpha$ small), this gives a factor-$\alpha$ improvement in gate count.

[[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]] find numerically optimised 8th-order formulas that are ~100× more accurate than standard Suzuki, by searching over-parameterised coefficient spaces.

---

## Where product formulas sit in the landscape

| Method | Gate complexity (schematic) | Precision dependence | Ancilla cost | Best regime |
|---|---|---|---|---|
| **Product formula ($2k$-th order)** | $O((\Gamma t)^{1+1/(2k)} / \varepsilon^{1/(2k)})$ | Polynomial | None | Structured / local $H$, moderate $\varepsilon$ |
| **Taylor series / LCU** | $O(\Gamma \|H\| t \cdot \mathrm{polylog}(1/\varepsilon))$ | Polylogarithmic | $O(\log(\Gamma t/\varepsilon))$ ancillae | Black-box, high precision |
| **QSP / Qubitization** | $O(\|H\|t + \log(1/\varepsilon))$ | Additive log | Block encoding overhead | Optimal asymptotics |
| **qDRIFT** | $O(\lambda^2 t^2 / \varepsilon)$ | $1/\varepsilon$ | None | Many small terms |

Product formulas have no ancilla overhead, simple circuit structure, and often win for moderate precision and structured Hamiltonians. QSP/qubitization wins asymptotically but requires block encoding machinery. In practice, the crossover depends heavily on the specific Hamiltonian — the commutator-scaling bounds from Childs et al. (2019) often push the crossover much further toward product formulas than naive analysis suggests.

---

## Key papers in this vault

### Foundations and error analysis
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd 1996]] — the original proposal
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders 2005]] — rigorous complexity bounds, near-optimal order selection
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2010]] — extension to time-dependent / ordered exponentials
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes|Wiebe, Berry, Høyer & Sanders 2011]] — companion algorithm paper
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu 2019]] — tight commutator-scaling bounds
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs & Wiebe 2013]] — product formulas for $e^{[A,B]t}$
- [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes|Chen, Childs et al. 2022]] — improved commutator formulas: 3rd-order base + $\sqrt{10}$-copy recursion; applications to lattice gauge theories
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes|Su, Huang & Campbell 2021]] — fermionic seminorm refinement

### Randomised product formulas
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes|Childs, Ostrander & Su 2019]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell 2019 (qDRIFT)]]
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes|Cho, Berry & Hsieh 2022]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann, Steudtner, Kueng, Kieferová & Eisert 2022]]
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn & Rall 2025]]

### Corrected and optimised formulas
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab et al. 2024]]
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. 2025]]

### Chemistry / physics applications
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. 2015]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran, Su, Childs & Wiebe 2021]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan et al. 2020]]

---

## Key trick cards

- [[Trotter Commutator-Scaling Bound]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Finite Nested-Commutator Expansion]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Suzuki Recursion for Ordered Exponentials]]
- [[Recursive Group-Commutator Product Formula]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Restricted Interaction Norms for Product Formulas]]
- [[Vector Norm Error Analysis for Product Formulas]]

---

## See also

- [[Hamiltonian simulation]] — broader context
- [[Hamiltonian Simulation — Comparison Tables]] — cost comparison across all methods
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP / Qubitization]] — the asymptotically optimal alternative
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]] — the linear-combination-of-unitaries framework that multi-product formulas require
