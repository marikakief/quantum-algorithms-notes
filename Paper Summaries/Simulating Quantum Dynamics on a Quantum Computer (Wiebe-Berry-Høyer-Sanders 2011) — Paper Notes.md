> **Source:** Nathan Wiebe, Dominic W. Berry, Peter Høyer, and Barry C. Sanders, *Simulating Quantum Dynamics on a Quantum Computer*, arXiv:1011.3489, J. Phys. A: Math. Theor. **44**, 445308 (2011)
> **Links:** [arXiv](https://arxiv.org/abs/1011.3489) · [Journal](https://doi.org/10.1088/1751-8113/44/44/445308)
> **Tags:** #hamiltonian-simulation #time-dependent #product-formulas #suzuki #oracle-model #sparse #adaptive-timestep

---

## The computational problem

Given a time-dependent Hamiltonian

$$
H(t) = \sum_{\mu=1}^{M} T_\mu^\dagger H_\mu(t)\, T_\mu,
$$

where each $H_\mu(t)$ is $d$-sparse in the computational basis and the $T_\mu$ are known unitary basis changes, simulate

$$
U(t_0 + \Delta t,\, t_0) = \mathcal{T}\exp\!\left(-i\int_{t_0}^{t_0+\Delta t} H(u)\,du\right)
$$

with operator-norm error $\leq \varepsilon$, using quantum oracles that provide Hamiltonian matrix elements at discrete times.

**Oracle model.** Two types of oracles:

- $Q_{\text{col}}(p, i, \mu)$: returns the $p$-th bit of the $i$-th potentially nonzero column index in a given row of $H_\mu$. Gives the sparsity structure.
- $Q_{\text{val}}(p, \mu, q)$: returns the $p$-th bit of a polar-form encoding of $[H_\mu(t_q)]_{xy}$, where $t_q = t_0 + (q - 1/2)\Delta t / 2^{n_t}$ is a uniform time mesh. Gives the numerical values at discrete times.

This is the full end-to-end algorithmic development of the product-formula approach to time-dependent sparse simulation: it takes the [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|mathematical framework from the 2010 companion paper]] and turns it into a concrete quantum algorithm with explicit resource counts including oracle precision, time discretization, and matrix-entry rounding.

---

## What the paper does

Takes the [[Suzuki Recursion for Ordered Exponentials|Suzuki recursion for ordered exponentials]] established in the companion paper (arXiv:0812.0562) and builds a complete quantum simulation algorithm on top of it — explicitly accounting for oracle bit-precision, time-mesh discretization, and matrix-value rounding, and extending the result to adaptive step sizes and Hamiltonians with isolated discontinuities.

The main advance over the 2010 paper is the move from pure mathematical bounds to a quantum algorithm: you get end-to-end complexity in terms of oracle calls, including the overhead from finite-precision arithmetic that the math paper swept under the rug.

The adaptive-timestep variant is genuinely useful: when $\|H(t)\|$ varies significantly over the simulation interval, using the instantaneous norm $\Upsilon(t)$ rather than the global maximum $\Lambda$ can save substantially.

---

## The algorithm / construction

### Sparse decomposition

Each $d$-sparse $H_\mu$ is decomposed via the [[Edge-Coloring Decomposition for Sparse Hamiltonians|BACS edge-coloring]] into at most $6d^2$ one-sparse Hamiltonians. Each 1-sparse term is then simulated via [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|2×2 block diagonalization]]: one oracle call locates the nonzero neighbor, and a controlled rotation applies the exact $2 \times 2$ exponential. The cost per 1-sparse simulation is $C$ oracle calls.

Total 1-sparse pieces: at most $M' = 6 M d^2$.

### Constant-timestep integrator (Theorem 5)

On $[t_0, t_0 + \Delta t]$, apply the order-$k$ Suzuki [[product formula]], splitting into $r$ equal subintervals as needed. The formula at each subinterval evaluates all Hamiltonian terms at the **midpoint** of the subinterval. Error budget: $\varepsilon/2$ from the integrator (Lemma 4), $\varepsilon/2$ from discretization (Lemma 1).

**Discretization precision requirements (Lemma 1):**

$$
n_t \geq \left\lceil \log_2\!\left(\frac{\max_\mu \|\partial_t H_\mu\|_{\max} \cdot 32kMd^2 (5/3)^{k-1} \Delta t^2}{\varepsilon}\right)\right\rceil
$$

$$
n_H \geq 2\left\lceil \log_2\!\left(\frac{32kMd^2 (5/3)^{k-1} \Lambda_{\max} \Delta t}{\varepsilon}\right)\right\rceil + 6
$$

These are the number of bits needed for the time mesh and matrix-value encoding respectively. See [[Discretization Budget Splitting for Sparse Simulation]].

**Query complexity (Lemma 4 / Theorem 5):** If $\{H_\mu\}$ is $\Lambda\text{-}2k$-smooth on $[t_0, t_0+\Delta t]$,

$$
\text{QueryCost} \leq 12C M d^2 5^{k-1} \left\lceil 24kd^2\Lambda\Delta t \left(\frac{5}{3}\right)^k \left(\frac{6d^2\Lambda\Delta t}{\tilde\varepsilon/2}\right)^{1/(2k)}\right\rceil,
$$

where $\tilde\varepsilon = \min\{\varepsilon,\, 18(5/3)^{k-1}d^2\Lambda\Delta t\}$ and $N_T \leq \text{QueryCost}/(3Cd^2)$ calls to the basis-change unitaries $\{T_\mu\}$.

### Handling discontinuities (Corollary 6)

If $H(t)$ has $L$ isolated discontinuity times $t_1,\ldots,t_L$ on $[t_0, t_0+\Delta t]$ but is $\Lambda\text{-}2k$-smooth elsewhere, excise $\delta$-neighborhoods around each $t_\ell$ and simulate the remainder. The omitted evolution contributes at most $e^{H_{\max}\delta} - 1 \approx H_{\max}\delta$ to the error. Set $\delta$ to use half the error budget. The query cost gains an additive $L$ factor:

$$
\text{QueryCost} \leq 12CMd^2 5^{k-1}\left[(L+1) + 24kd^2\Lambda\Delta t\left(\frac{5}{3}\right)^k\left(\frac{6d^2\Lambda\Delta t}{\varepsilon/3}\right)^{1/(2k)}\right].
$$

See [[Discontinuity Excision for Hamiltonian Simulation]].

### Adaptive-timestep integrator (Theorem 8)

Replace the global smoothness parameter $\Lambda$ with a time-varying bound $\Upsilon(t)$ (a [[Pointwise-Smoothness Parametrization|pointwise-smooth parametrization]]) satisfying $|\partial_t \Upsilon(t)| \leq K^2 [\Upsilon(t)]^2$. Subinterval widths are chosen so that

$$
\max_{t\in[t_p, t_{p+1}]}\Upsilon(t)\cdot(t_{p+1}-t_p)\leq\frac{(\varepsilon/r)^{1/(2k+1)}}{24d^2k(5/3)^{k-1}}.
$$

The integration points $\{t_p\}$ are determined adaptively. The query cost scales with the **average** $\bar\Upsilon$ over $[t_0, t_0+\Delta t]$, not the maximum:

$$
\text{QueryCost} \leq 12CMd^2 5^{k-1}\left\lceil\left[24d^2k\left(\frac{5}{3}\right)^{k-1}\bar\Upsilon\Delta t\right]^{1+1/(2k)}(\varepsilon/4)^{-1/(2k)} + 3K^2\bar\Upsilon\Delta t + 1\right\rceil.
$$

This is strictly better than the constant-step bound whenever $\|H(t)\|$ is concentrated in a small portion of $[0,\Delta t]$.

See [[Adaptive Timestep via Local Smoothness Control]].

---

## Key results

**Lemma 1** (discretization precision): sets $n_t$ and $n_H$ bits to guarantee round-off error $\leq \varepsilon/2$ from finite-precision time and matrix-value oracles. Both grow logarithmically in the relevant norms and as $\log(1/\varepsilon)$.

**Theorem 5** (constant-step simulation): combines integrator error (Lemma 4) and discretization error (Lemma 1) into a single end-to-end bound. QueryCost scales as

$$
O\!\left(CMd^2 5^{k-1} d^2 k \Lambda\Delta t \cdot \left(\frac{d^2\Lambda\Delta t}{\varepsilon}\right)^{1/(2k)}\right).
$$

Optimizing $k$ over $\Lambda, \Delta t, \varepsilon$ recovers near-linear scaling in $\Delta t$ for $\Lambda$-smooth $H$.

**Corollary 6** (discontinuous $H$): $L$ isolated discontinuities add an $O(L)$ term to the query cost; the smooth-simulation cost is otherwise unchanged.

**Theorem 8** (adaptive-step simulation): query cost driven by $\bar\Upsilon$ (time-average of pointwise norm bound) rather than $\Lambda_{\max}$ (global maximum). Can give significant speedup when $\|H(t)\|$ is bursty or highly localized.

---

## Comparison with prior work

| Paper | Setting | What's accounted for | Cost parameter |
|---|---|---|---|
| [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes\|Wiebe-Berry-Høyer-Sanders (2010)]] | Math bounds on ordered exponentials | Integrator error only | $\Lambda$-smoothness; $N$ exponentials |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes\|Berry et al. (2005)]] | Time-independent sparse | Integrator + oracle calls | $d^4 \|H\|_{\max} t$ |
| [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes\|Berry-Childs (2011)]] | Time-independent black-box | Query complexity | Early linear-$t$ gains |
| **This paper (2011)** | Time-dependent sparse | Integrator + discretization + oracle precision | $\Lambda$ or $\bar\Upsilon$, adaptive |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes\|Kieferová-Scherer-Berry (2018)]] | Time-dependent sparse (LCU) | Full query count | $d^2 H_{\max} t \cdot \text{polylog}(1/\varepsilon)$ |

The 2018 Dyson-series paper supersedes this one in precision scaling (polylog vs polynomial in $1/\varepsilon$), but this paper's adaptive-timestep variant remains relevant when the time-varying norm $\Upsilon(t)$ is much smaller on average than its peak — in that regime the averaging advantage can beat the LCU overhead in practice.

---

## Limits / caveats

- **Smoothness required.** Order-$k$ simulation requires $H$ to have $2k$ bounded derivatives. No smoothness → no order $> 1$. (Same caveat as the 2010 paper.)
- **Precision scaling is polynomial.** Unlike Dyson/LCU methods (polylog in $1/\varepsilon$), the cost here scales as $\varepsilon^{-1/(2k)}$. For high-precision requirements this can be costly.
- **$5^{k-1}$ constant blowup.** The number of exponentials at order $k$ grows as $5^{k-1}$; the optimization over $k$ balances this against the $\varepsilon^{-1/(2k)}$ gain. The optimal $k$ grows logarithmically in $\Lambda\Delta t/\varepsilon$.
- **Oracle precision.** The $n_H$ and $n_t$ bit-length requirements are logarithmic in $1/\varepsilon$ but depend on derivative norms — if $\|\partial_t H\|$ is large, you need a fine time mesh, which affects both oracle design and gate counts.
- **Adaptive step requires regularity of $\Upsilon(t)$.** The condition $|\partial_t\Upsilon| \leq K^2 \Upsilon^2$ rules out Hamiltonians with very rapid norm spikes.
- **Predates block-encoding/QSP.** This framework assumes a specific sparse oracle model; the cleaner modern approach via [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] gets optimal precision scaling but requires a block-encoding of $H$, which in the time-dependent case involves extra overhead for time ordering.

---

## Reusable ideas

1. [[Discretization Budget Splitting for Sparse Simulation]] — partition the total error budget as $\varepsilon = \varepsilon_{\text{integrator}} + \varepsilon_{\text{round-off}}$ (each $= \varepsilon/2$), then independently derive oracle precision requirements for each half. Keeps analysis clean and separable.
2. [[Adaptive Timestep via Local Smoothness Control]] — use a pointwise-smooth bound $\Upsilon(t)$ in place of a global $\Lambda$; choose step sizes so the local Suzuki error is $\leq \varepsilon/r$ on each sub-interval. Total cost driven by $\bar\Upsilon$ (the time-average) rather than $\Lambda_{\max}$ — can be much cheaper when $\|H(t)\|$ is concentrated.
3. [[Discontinuity Excision for Hamiltonian Simulation]] — handle isolated discontinuities by removing $\delta$-neighborhoods and bounding the omitted evolution as $\|e^{iH_{\max}\delta} - I\| \leq H_{\max}\delta$; simulate the smooth remainder normally. Adds only $O(L)$ to complexity for $L$ discontinuities.

---

## References within this paper

- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders (2010)]] — the companion theory paper (arXiv:0812.0562) proving the Suzuki recursion and smoothness-order saturation results that this paper uses as its core integrator
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — the sparse 1-sparse decomposition (BACS) and edge-coloring used here; time-independent predecessor
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — foundational product-formula simulation
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin, Qarry, Somma & Verstraete (2011)]] — concurrent work; derivative-free Trotter for time-dependent simulation (different approach: integrates coefficients rather than evaluating at midpoints)
- Berry & Childs (2009), arXiv:0910.4157 — black-box [[Hamiltonian simulation]] via sparse oracles; informs the oracle model here
- Suzuki (1991) — recursive product-formula construction extended in the 2010 companion paper

---

## Cross-links

### Paper notes
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — companion theory paper; proved the integrator bounds used here
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — time-independent predecessor; BACS decomposition
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — supersedes for precision scaling; uses Dyson/LCU approach instead
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — further development in interaction picture
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — concurrent alternative approach
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — contemporary sparse simulation result
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — later sharpening of Trotter error using nested commutators
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]] — later improvement using vector-norm bounds

### Trick cards
- [[Suzuki Recursion for Ordered Exponentials]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Λ-Smoothness Parametrization]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]]
- [[Discretization Budget Splitting for Sparse Simulation]]
- [[Adaptive Timestep via Local Smoothness Control]]
- [[Discontinuity Excision for Hamiltonian Simulation]]
