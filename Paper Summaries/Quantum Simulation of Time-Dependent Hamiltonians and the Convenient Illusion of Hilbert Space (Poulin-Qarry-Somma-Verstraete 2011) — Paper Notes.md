> **Source:** David Poulin, Angie Qarry, Rolando D. Somma, and Frank Verstraete, *Quantum simulation of time-dependent Hamiltonians and the convenient illusion of Hilbert space*, arXiv:1102.1360, Phys. Rev. Lett. **106**, 170501 (2011)
> **Links:** [arXiv](https://arxiv.org/abs/1102.1360) · [PRL](https://doi.org/10.1103/PhysRevLett.106.170501)
> **Tags:** #hamiltonian-simulation #time-dependent #trotter #counting-argument #complexity-theory #product-formulas

---

## The computational problem

Given an $N$-particle system with local Hamiltonian $H(t) = \sum_{X} H_X(t)$ where each term acts on at most $k$ bodies with bounded norm $\|H_X(t)\| \le E$, and the time-dependence of $H(t)$ is **completely arbitrary** (no smoothness assumptions), can the time-evolution operator $U(0,T) = \mathcal{T}\exp\{-i\int_0^T H(s)\,ds\}$ be efficiently simulated by a quantum circuit?

Equivalently: how large is the set of states reachable from a fiducial state by polynomial-time local Hamiltonian evolution?

## What the paper does

Two results, both about time-dependent Hamiltonians with no restriction on how fast they can change:

1. **Simulation result.** Any local time-dependent Hamiltonian can be simulated by a polynomial-size quantum circuit, even when $H(t)$ varies arbitrarily fast. The key is a time-dependent [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter-Suzuki decomposition]] (due to Huyghebaert and De Raedt) whose error depends on commutator norms, not on derivatives $\|\partial H/\partial t\|$. This is the first simulation result that doesn't require smoothness.

2. **Counting result.** The manifold of states reachable by polynomial-time local evolution occupies an exponentially small fraction of Hilbert space. This is the quantum analogue of Shannon's counting argument for Boolean functions. Most states in Hilbert space are "unphysical" — they require exponential time to prepare.

The second result follows from the first via [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev]] discretisation of the polynomial-size circuit into gates from a finite set, then counting the number of distinct circuits of that size.

## The algorithm / construction

### Time-dependent Trotter-Suzuki decomposition

Break $[0,T]$ into $n$ intervals of length $\Delta t$. On each interval, decompose the Hamiltonian's $L$ terms using the [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] formula. For two terms $H_1, H_2$:

$$U_{\mathrm{TS}}(t_j, t_j + \Delta t) = \mathcal{T}\exp\!\left(-i\int_{t_j}^{t_j+\Delta t} H_1(s)\,ds\right) \cdot \mathcal{T}\exp\!\left(-i\int_{t_j}^{t_j+\Delta t} H_2(s)\,ds\right)$$

with error bounded by:

$$\|U(t_j, t_j+\Delta t) - U_{\mathrm{TS}}(t_j, t_j+\Delta t)\| \le \frac{1}{2} c_{\max}^2\,(\Delta t)^2$$

where $c_{\max} = \max_t \max_X \|H_X(t)\|$. This is a simplified bound; the tighter version from Huyghebaert-De Raedt involves the commutator $\|[H_1, H_2]\|$ instead of $c_{\max}^2$ (see the [[Generalised Trotter for Time-Dependent Hamiltonians|trick card]]). Either way, the bound depends **only on operator norms** (or commutator norms), **not** on any derivatives $\|\partial H/\partial t\|$. This is the essential difference from all prior product-formula approaches.

For $L$ terms, iterate the two-term decomposition $\log_2 L$ times. The total error per step is bounded by $\frac{1}{2}c_{\max}^2 L^2 (\Delta t)^2$.

### Gate count

To achieve total error $\varepsilon/2$ from the Trotter decomposition, choose:

$$\Delta t = \frac{\varepsilon}{c_{\max}^2 L^2 T}$$

giving $G(\varepsilon, L) = c_{\max}^2 T^2 L^3 / \varepsilon$ many $k$-body gates. Each $k$-body gate is then compiled into standard 2-qubit gates via [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev]], adding a $d_{\mathrm{SK}} \log(c_{\mathrm{SK}} G / \varepsilon)$ factor per gate. Total gate count:

$$G_{\mathrm{tot}}(\varepsilon, T) = d_{\mathrm{SK}} \cdot G(\varepsilon, T) \cdot \log\!\left(c_{\mathrm{SK}} \frac{G(\varepsilon, T)}{\varepsilon}\right)$$

which is $\mathrm{poly}(N, T, 1/\varepsilon)$. This proves that time-dependent local Hamiltonian evolution is in BQP.

### The counting argument

The number of states reachable by circuits of size $G_{\mathrm{tot}}$ from a universal gate set of size $M$ on $K$ qubits is at most $(MK^2)^{K^\alpha}$ (for some constant $\alpha$ from the polynomial gate count). Place an $\varepsilon$-ball around each circuit output in Hilbert space. The total volume covered is $O(K^K \varepsilon^{2K})$ of the full $2^K$-dimensional sphere — exponentially small in $K$.

This is the quantum Shannon argument: just as most Boolean functions have no polynomial-size classical circuits (Shannon: $2^{\mathrm{poly}(N)}$ circuits vs $2^{2^N}$ functions), most quantum states have no polynomial-time local Hamiltonian preparation (polynomially many circuit outputs vs an exponentially high-dimensional state space).

### Randomised [[Product Formulas]] (bonus result)

The time-dependent Trotter decomposition above produces time-ordered exponentials $\mathcal{T}\exp(-i\int H_X(s)\,ds)$, not simple exponentials $\exp(-iH_X(t)\Delta t)$. For practical simulation, the paper shows how to recover a genuine [[Product Formulas]] using randomness:

1. **Drop the time-ordering.** For a single operator-valued term $H_X(t)$, the time-ordered exponential differs from the ordinary exponential of the integral by $O(\|H_X\|^2 \Delta t)$ — linear in $\Delta t$ (Eq. (4) / Appendix A). Note: if the term has the form $f_X(t) H_X$ (scalar coefficient, fixed operator), then $[H_X(u), H_X(v)] = 0$ and the time-ordering is exact — zero error.

2. **Monte Carlo integration.** Approximate $\bar{H}_{X,j} = \frac{1}{\Delta t}\int_{t_j}^{t_j+\Delta t} H_X(s)\,ds$ by sampling $m$ random times $\tau_j^k$ from the interval and averaging: $\bar{H}_{X,j} \approx \frac{1}{m}\sum_{k=1}^m H_X(\tau_j^k)$. Error: $O(\|H_X\|/\sqrt{m})$.

3. **Standard Trotter on the samples.** Apply $\prod_k \exp(-i\frac{\Delta t}{m} H_X(\tau_j^k))$, which is a product of ordinary exponentials at specific time points.

Result: a standard product formula for arbitrary time-dependent $H(t)$, at the cost of randomness and Monte Carlo sampling overhead.

## Key results

**Theorem (informal).** For any $k$-local time-dependent Hamiltonian on $N$ particles with $L = \mathrm{poly}(N)$ terms and $\|H_X(t)\| \le E$, the time evolution $U(0,T)$ can be approximated to error $\varepsilon$ by a quantum circuit of size $\mathrm{poly}(N, T, 1/\varepsilon)$, with **no assumptions on the time-dependence of $H(t)$**.

**Corollary.** Hamiltonian-based quantum computation with arbitrarily rapidly changing local Hamiltonians is no more powerful than the standard quantum circuit model. The two models are polynomially equivalent.

**Corollary.** The manifold of states reachable by polynomial-time local Hamiltonian evolution is exponentially small in $2^N$-dimensional Hilbert space.

## Comparison with prior work

| Feature | Standard Trotter (e.g. Lloyd 1996) | Prior time-dependent methods | This paper |
|---|---|---|---|
| Time-independent $H$ | ✓ | ✓ | ✓ |
| Smooth time-dependence | ✓ (trivially) | ✓ | ✓ |
| Arbitrary time-dependence | ✗ (error $\sim \|\partial H/\partial t\|$) | ✗ | ✓ |
| Error bound depends on $\|\partial H/\partial t\|$? | N/A | Yes | **No** |
| Practical [[Product Formulas]]? | Yes | Yes | Only via randomisation |

The paper fills a conceptual gap: prior to this work, it was widely believed but not formally proved that fast-changing Hamiltonians couldn't enable computation beyond BQP. The derivative-free error bound from the [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] makes this rigorous.

## Limits / caveats

- The gate count is $O(L^3 T^2 / \varepsilon)$ — cubic in the number of terms. This is significantly worse than modern methods ([[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]], [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Taylor-LCU]]) which achieve near-linear scaling and polylogarithmic precision dependence. The paper's value is existential (proving the polynomial bound exists), not algorithmic (providing the best bound).

- The Trotter decomposition produces time-ordered exponentials, not standard gates. The randomised [[Product Formulas]] that recovers standard gates adds Monte Carlo overhead and only converges in expectation.

- The counting argument gives an *existence* result about unreachable states but says nothing about *which* states are unreachable or how to decide whether a specific state is physical.

- The paper only considers $k$-local Hamiltonians with bounded norm. Systems with unbounded operators (e.g. position/momentum on a lattice) are not covered. See [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes|An-Fang-Liu (2021)]] for that setting.

- The model assumes oracular access to $H(t)$ at any time. The paper doesn't address the classical cost of specifying or evaluating a rapidly changing Hamiltonian.

## Reusable ideas

1. [[Generalised Trotter for Time-Dependent Hamiltonians]] — the Huyghebaert–De Raedt decomposition that eliminates derivative dependence from Trotter error. Already has a trick card in the vault (sourced to Huyghebaert–De Raedt 1990 and An-Fang-Liu 2021); this paper is its first major application to a complexity-theoretic question. **Note:** the vault trick card covers the scalar-coefficient case $f_j(t)H_j$, where the per-term exponentials are ordinary (commuting). This paper uses the general operator-valued $H_X(t)$ version, where per-term factors are *time-ordered* exponentials — recovering ordinary exponentials then requires the Monte Carlo trick below.

2. [[Monte Carlo Average-Hamiltonian Product Formula]] — replacing time-ordered exponentials with Monte Carlo sampling of the average Hamiltonian, then applying standard Trotter on the samples. Converts any time-dependent simulation into a randomised [[Product Formulas]] without smoothness assumptions.

3. [[Quantum Shannon Counting Argument]] — the quantum version of Shannon's classical circuit counting: combine a poly-size circuit simulation result with Solovay-Kitaev discretisation to show that reachable states are exponentially sparse in Hilbert space.

## References within this paper

- Huyghebaert and De Raedt (1990) — the time-dependent Trotter-Suzuki formula that removes derivative dependence; the technical engine of this paper
- Shannon (1948) — classical counting argument for Boolean functions; the template for the quantum version
- [[Solovay-Kitaev Recursive Gate Compilation]] — used to discretise continuous gates into a finite gate set for the counting argument
- Feynman (1982) — original proposal for quantum simulation
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — first product-formula quantum simulation
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — higher-order [[Product Formulas]]s for sparse Hamiltonians
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Temme-Osborne-Vollbrecht-Poulin-Verstraete (2011)]] — contemporary work by overlapping authors
- Wiebe, Berry, Høyer, Sanders (2010) — [[Product Formulas]]s with derivative-dependent complexity that this paper improves upon

## Cross-links

### Paper notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]]
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]

### Trick cards
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[Monte Carlo Average-Hamiltonian Product Formula]]
- [[Quantum Shannon Counting Argument]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
