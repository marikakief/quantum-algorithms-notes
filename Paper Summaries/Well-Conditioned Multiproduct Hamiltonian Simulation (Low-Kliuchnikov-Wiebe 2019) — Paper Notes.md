> **Source:** Guang Hao Low, Vadym Kliuchnikov, Nathan Wiebe, *Well-conditioned multiproduct Hamiltonian simulation*, arXiv:1907.11679 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1907.11679)
> **Tags:** #hamiltonian-simulation #multi-product #product-formulas #LCU #richardson-extrapolation

---

## The computational problem

Approximate the time-evolution operator $e^{-iHt}$ for $H = \sum_{j=1}^N h_j$ using quantum gates. The cost measures are: queries to a base [[Product Formulas]] $U_2$ (the symmetric second-order Trotter formula), total gate count, and the dependence on simulation time $t$ and target error $\epsilon$.

The goal: combine the *system-size* advantages of [[Product Formulas]]s (which exploit commutativity between Hamiltonian terms and scale almost-linearly in system size for local interactions) with the *time-and-error* advantages of [[Linear Combination of Unitaries (LCU)|LCU]] approaches (near-linear in $t$, logarithmic in $1/\epsilon$).

---

## What the paper does

Constructs **well-conditioned multiproduct formulas** — linear combinations of [[Product Formulas]]s at different step sizes — that achieve near-optimal $O(t\lambda \log^2(t\lambda/\epsilon))$ query complexity while inheriting the commutator-dependent scaling of the base product formula. The key technical result is a super-exponential reduction in the condition number $\|\vec{a}\|_1$ from $e^{\Omega(m)}$ (prior multiproduct formulas) to $O(\log m)$, via an elegant connection to Chebyshev polynomials. This is the paper that made multiproduct formulas practical — both classically and quantumly.

This work is the direct precursor to [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]], which replaces the coherent [[Linear Combination of Unitaries (LCU)|LCU]] implementation with randomized sampling — eliminating the ancilla overhead entirely while keeping the error cancellation structure.

---

## Background: the multiproduct conditioning problem

A standard order-$2m$ integrator via the Suzuki recursion (Eq. 2) makes $5^{m-1}$ queries to $U_2$ — exponential in the order. The idea behind multiproduct formulas is to get high-order accuracy cheaply: take a *linear combination* of the base formula $U_2$ evaluated at different step sizes, and choose coefficients to cancel error terms via a Vandermonde system.

Any symmetric [[Product Formulas]] has a BCH expansion $U_2(\Delta) = e^{-iH\Delta + E_3\Delta^3 + E_5\Delta^5 + \cdots}$, so
$$U_2^{k_j}(\Delta/k_j) = e^{-iH\Delta} + \frac{\Delta^3}{k_j^2}\tilde{E}_3(\Delta) + \frac{\Delta^5}{k_j^4}\tilde{E}_5(\Delta) + \cdots$$

The multiproduct formula
$$U_{\vec{k}}(\Delta) = \sum_{j=1}^M a_j U_2^{k_j}\!\left(\frac{\Delta}{k_j}\right) = e^{-iH\Delta} + O(\Delta^{2m+1})$$
cancels all error terms up to order $2m$ if the coefficients $\vec{a}$ solve the Vandermonde system
$$V_{m,M}(\vec{k}^{-2})\,\vec{a} = \hat{e}_1$$

This is a generalisation of [[Richardson Extrapolation over Randomized Step Sizes|Richardson extrapolation]] from classical numerical analysis (Chin 2010). The simplest choice $k_j = j$ (arithmetic progression) gives polynomial query cost $\|\vec{k}\|_1 \in O(m^2)$ — but the coefficients are exponentially ill-conditioned: $\|\vec{a}\|_1 \in e^{\Omega(m)}$.

In the quantum setting, the multiproduct is implemented via [[Linear Combination of Unitaries (LCU)|LCU]]: prepare a coefficient state $|a\rangle = \sum_j \sqrt{a_j}|j\rangle$, apply a selector $S = \sum_j |j\rangle\langle j| \otimes \text{sign}(a_j)\, U_2^{k_j}(\Delta/k_j)$, and postselect. Success probability $\sim 1/\|\vec{a}\|_1^2$, boosted by [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]] at cost $O(\|\vec{a}\|_1)$. With $\|\vec{a}\|_1 \in e^{\Omega(m)}$, this kills the advantage entirely.

---

## The algorithm / construction

### Step 1: Chebyshev-node multiproduct formulas (the conditioning fix)

The central insight: the Vandermonde system's conditioning depends on the *choice of exponents* $\vec{k}$, not just their number. By choosing exponents related to Chebyshev interpolation points, the condition number drops from exponential to logarithmic.

**Construction.** Consider Chebyshev polynomials $T_j(x) = \cos(j\cos^{-1}(x))$ and the Chebyshev interpolation points
$$x_j^{(m)} = \sin^2\!\left(\frac{\pi(2j-1)}{4m}\right) = 1/k_j''^{\,2}$$

These define real-valued exponents $k_j''$ with closed-form coefficients:
$$a_j''^{(m)} = \frac{(-1)^{j+1}}{m}\cot\!\left(\frac{\pi(2j-1)}{4m}\right), \quad j \in [m]$$

The orthogonality of Chebyshev polynomials over the interpolation points directly yields the Vandermonde solution — the key connection. The resulting bounds are:
- $\|\vec{k}''\|_1 \in \Theta(m\log m)$ (exponent sum)
- $\|\vec{a}''\|_1 \in \Theta(\log m)$ (condition number)

See [[Chebyshev-Node Conditioning for Multiproduct Formulas]] for the standalone trick.

### Step 2: Rounding to integer exponents

Real-valued exponents $k_j''$ are impractical (implementing fractional queries to $U_2$ requires additional overhead). The paper rounds to integers via:

1. Take an intermediate solution $k_j' = 1/\sqrt{x_j^{(2m)}}$ using only the first $m$ of $2m$ Chebyshev points — this increases the gap between consecutive exponents.
2. Scale by $K < \sqrt{8m}/\pi$ and round: $k_j = \lceil K k_j'\rceil$.
3. The fractional shift $|k_j - Kk_j'|/m \in \Theta(1/m)$ is small enough that the condition number changes by at most a constant factor (proved via a careful perturbation argument using Eq. 5).

Result: integer exponents with $\|\vec{k}\|_1 \in O(m^2\log m)$ and $\|\vec{a}\|_1 \in O(\log m)$.

### Step 3: Optimal numerical construction via linear programming

For practical use, the paper also provides a linear program (Eq. 12):
$$\min_{\vec{a}} \|\vec{a}\|_1 \quad \text{s.t.} \quad V_{m,M}(\vec{k}^{-2})\cdot\vec{a} = \hat{e}_1, \quad k_j = j \in [M]$$

This finds multiproduct formulas with optimal condition number for any given maximum exponent $M$. The $\ell_1$-minimisation also enforces sparsity (exactly $m$ non-zero coefficients), analogous to compressed sensing. Solutions for orders $m = 2$ through $m = 15$ are tabulated in the appendices.

### Step 4: LCU implementation with OAA

A single multiproduct step is implemented as an [[Linear Combination of Unitaries (LCU)|LCU]] circuit:
- **PREPARE:** $|a\rangle = \sum_j \sqrt{|a_j|}\,|j\rangle$ (cost: $O(m)$ gates)
- **SELECT:** $\sum_j |j\rangle\langle j| \otimes \text{sign}(a_j)\,U_2^{k_j}(\Delta/k_j)$ (cost: $\|\vec{k}\|_1$ queries to $U_2$)
- Postselection succeeds with probability $1/\|\vec{a}\|_1^2$
- [[Oblivious Amplitude Amplification (Robust)|Oblivious amplitude amplification]] boosts success to $1 - O(\epsilon/r)$ at multiplicative cost $O(\|\vec{a}\|_1)$

Multiple steps ($r$ of them) are composed, each boosted independently. Total success probability: $1 - O(\epsilon)$.

### Step 5: Order optimisation

The order $2m$ is not fixed — it's chosen to minimise total cost. Setting $2m \approx e^{W(\log(t\lambda/\epsilon))}$ via the Lambert-$W$ function (where $ze^z = \log(t\lambda/\epsilon)$ by definition), the number of steps simplifies to $r \in \Theta(t\lambda)$ and the per-step query cost to $O(\log^2(t\lambda/\epsilon))$. See [[Order-Optimized Multiproduct Simulation]].

---

## Key results

**Theorem 1 (Well-conditioned multiproduct formulas).** There exist order-$2m$ multiproduct formulas with polynomial integer exponents $\|\vec{k}\|_1 \in O(m^2\log m)$ and logarithmic condition number $\|\vec{a}\|_1 \in O(\log m)$.

**Theorem 2 (Hamiltonian simulation).** Time evolution $e^{-iHt}$ can be approximated to error $\epsilon$ by a quantum circuit using
$$O\!\left(t\lambda\log^2\!\left(\frac{t\lambda}{\epsilon}\right)\right)$$
controlled-$U_2$ queries and $O(t\lambda\log(t\lambda/\epsilon))$ additional gates, where $\lambda = \sum_j \|h_j\|$.

**Theorem 3 (Commutator dependence).** The error of an order-$2m$ multiproduct formula with $U_\alpha$ as base sequence depends on Hamiltonian terms only through nested commutators $[h_{j_\beta}, [\cdots, [h_{j_2}, h_{j_1}]\cdots]]$ of depth $\beta > \alpha$.

This commutator dependence is what gives multiproduct formulas their system-size advantage over generic [[Linear Combination of Unitaries (LCU)|LCU]] methods, which scale with $\lambda = \sum_j \|h_j\|$ regardless of commutativity.

---

## Comparison with prior work

| Method | Time-error scaling | System-size scaling | Exploits commutativity? |
|---|---|---|---|
| Suzuki order-$2m$ (Eq. 2) | $O(t^{1+1/2m}\epsilon^{-1/2m})$ | $O(N^{1+o(1)})$ for local $H$ | ✓ |
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes\|Truncated Taylor series]] | $O(t\,\text{polylog}(t/\epsilon))$ | $O(\lambda) = O(N^2)$ in general | ✗ |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes\|QSP]] | $O(t + \log(1/\epsilon))$ | $O(\lambda) = O(N^2)$ in general | ✗ |
| Ill-conditioned MPF (Childs-Wiebe 2012) | $O(t\,\text{polylog}(t/\epsilon))$ in principle | Inherits from base PF | Exponentially small success prob. |
| **This paper (LKW 2019)** | $O(t\log^2(t/\epsilon))$ | Inherits from base PF | ✓ |

The unique selling point: this is the only method that simultaneously achieves near-optimal $t/\epsilon$ scaling *and* exploits commutativity. [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] is asymptotically better in $t$ and $\epsilon$ but pays $O(\lambda)$ regardless of how commutative the system is. For geometrically local systems where $\sum_{j<k}\|[h_j, h_k]\| \ll \lambda^2$, multiproduct formulas can win.

---

## Limits / caveats

1. **Ancilla overhead.** The [[Linear Combination of Unitaries (LCU)|LCU]] implementation requires $O(\log m)$ ancilla qubits and [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]]. This is eliminated by the randomised approach of [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann et al. (2022)]].

2. **Worst-case vs commutator bounds.** Theorem 3 shows commutator dependence exists but doesn't give tight commutator bounds. The paper acknowledges this: "rigorous error bounds on the nested commutators would be of practical relevance." The tight [[Trotter Commutator-Scaling Bound|commutator-scaling bounds]] of [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] apply to the base product formula, and propagate into the multiproduct error, but the multiproduct-specific commutator bounds remain looser than one would like.

3. **Constant factors.** The $O(t\lambda\log^2(t\lambda/\epsilon))$ bound hides constant factors. In the numerical benchmarks (1D Heisenberg chain), the multiproduct formula shows clear logarithmic scaling with error and near-linear scaling with system size, but the crossover with Suzuki formulas depends on the target error regime.

4. **State evolution only.** The deterministic version produces the full quantum state $e^{-iHt}|\psi\rangle$. For observable estimation, the randomised variant ([[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann et al. 2022]]) is typically preferred because it avoids the LCU/OAA overhead entirely.

5. **Not optimal in all parameters.** [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] achieves $O(t\|H\| + \log(1/\epsilon)/\log\log(1/\epsilon))$ which is strictly better in the $\epsilon$ dependence. The multiproduct formula trades a $\log^2$ factor for commutativity sensitivity.

---

## Reusable ideas

1. **[[Chebyshev-Node Conditioning for Multiproduct Formulas]]** — Use Chebyshev interpolation points as Vandermonde exponents to achieve logarithmic condition number. Applies to any context where you need well-conditioned linear combinations that cancel error terms order-by-order.

2. **[[Order-Optimized Multiproduct Simulation]]** — Vary the multiproduct order sub-logarithmically with $t/\epsilon$ (via the Lambert-$W$ function) to convert the per-step polynomial cost into an overall polylogarithmic overhead. A general strategy for any family of approximants parametrised by order.

3. **LP-based coefficient optimisation.** Minimise $\|\vec{a}\|_1$ subject to the Vandermonde constraints via a rational linear program. The $\ell_1$ minimisation simultaneously finds the sparsest solution (fewest non-zero terms) and the best-conditioned one. This is a reusable recipe for any LCU construction where the coefficients are determined by a linear system. (Closely related to the [[Resolution Factor Minimization for Multi-Product Formulas]] approach in the randomised setting.)

---

## Numerical benchmarks

The paper validates on the 1D Heisenberg chain $H = \sum_j (X_jX_{j+1} + Y_jY_{j+1} + Z_jZ_{j+1})$ with periodic boundary conditions.

- **Error scaling (Fig. 2 left):** Total $U_2$ query cost (including OAA) as a function of target error for $N = 10$ sites, $t = N$. Multiproduct formulas show clear logarithmic scaling: $\sim 90\log^{1.14}(1/\epsilon)$ vs product formulas at $\sim 132\,e^{0.23\log^{0.91}(1/\epsilon)}$.
- **System-size scaling (Fig. 2 middle/right):** Cost at fixed error $\epsilon = 10^{-8}$ as a function of system size. Using $U_2$ as base, cost scales as $\sim N^{1.25}$ to $N^{2.1}$ depending on multiproduct order. Using $U_4$ as base, the scaling improves to $\sim N^{1.08}$ to $N^{1.46}$.

The system-size scaling is the headline: it tracks the base product formula's scaling, not the generic $O(N^2)$ of [[Linear Combination of Unitaries (LCU)|LCU]] methods.

---

## Subsequent work and influence

This paper sits at a branching point in the Hamiltonian simulation literature:

- **[[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]]** directly builds on this work, replacing the coherent LCU with randomized sampling. The well-conditioned coefficients from LKW19 keep the resolution factor $\Xi = \|\vec{a}\|_1$ small, which directly controls the shot overhead $\Xi^4/\epsilon^2$. Without the conditioning result of this paper, randomized multiproduct formulas would be impractical. This is Marika's paper — the connection is tight.

- **[[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes|Watson (2025)]]** applies [[Richardson Extrapolation over Randomized Step Sizes|Richardson extrapolation over randomised step sizes]], citing LKW19's conditioning results to keep the extrapolation weights bounded.

- **[[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes|Cho-Berry-Hsieh (2022)]]** and **[[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn-Rall (2025)]]** represent alternative paths to achieving better $\epsilon$-scaling via randomisation — at the polynomial/QSP level rather than the product-formula level.

- **[[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab et al. (2024)]]** takes a different approach (boundary-injected commutator correctors) to improve product formula accuracy without going to multiproduct.

---

## References within this paper

- **Chin (2010)** — Multi-product splitting and Runge-Kutta-Nyström integrators. The classical numerical analysis origin of multiproduct formulas. Chin introduced the arithmetic-progression exponents $k_j = j$ with exponentially ill-conditioned coefficients.
- **[[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]]** — First coherent LCU for [[Hamiltonian simulation]]; also proposed multiproduct formulas but with $\|\vec{a}\|_1 \in e^{\Omega(m)}$, making the quantum implementation impractical.
- **[[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2014)]]** — [[Oblivious Amplitude Amplification (Robust)|Robust OAA]], used here to boost the LCU success probability.
- **[[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]]** — QSVT framework; provides the OAA machinery and the programmable rotation optimisation in Appendix B.
- **[[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]]** — Tight [[Trotter Commutator-Scaling Bound|commutator-scaling bounds]] for product formulas. The multiproduct formula inherits these bounds through the base sequence.
- **Lam (1998)** — Decomposition theorem for time-ordered products into nested commutators. Used in the proof of Theorem 3 (commutator dependence).
- **[[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe-Berry-Høyer-Sanders (2010)]]** — Higher-order product formulas for time-dependent Hamiltonians.
- **[[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]]** — QSP-based simulation; the near-optimal comparator.

---

## Cross-links

### Paper notes
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — randomises this paper's multiproduct approach
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the QSP competitor
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — commutator-scaling analysis of the base product formula
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — alternative randomisation at the QSP level
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — randomised order-doubling for product formulas
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — Richardson extrapolation over qDRIFT using LKW19 conditioning
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — original ill-conditioned multiproduct proposal
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — alternative product-formula improvement

### Trick cards
- [[Chebyshev-Node Conditioning for Multiproduct Formulas]] — the core technical trick
- [[Order-Optimized Multiproduct Simulation]] — order selection via Lambert-$W$
- [[Linear Combination of Unitaries (LCU)]] — implementation primitive
- [[Order-Condition Cancellation in Product Formulas]] — the error structure exploited by multiproducts
- [[Permutation Averaging for Product-Formula Error Cancellation]] — alternative error cancellation via randomisation
- [[Oblivious Amplitude Amplification (Robust)]] — probability boosting for the LCU step
- [[Richardson Extrapolation over Randomized Step Sizes]] — the incoherent analogue
- [[Randomized Multi-Product Sampling]] — the randomised implementation
- [[Resolution Factor Minimization for Multi-Product Formulas]] — coefficient optimisation in the randomised setting
