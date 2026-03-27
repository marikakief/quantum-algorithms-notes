> **Source:** John M. Martyn, Patrick Rall, *Halving the Cost of Quantum Algorithms with Randomization*, arXiv:2409.03744, npj Quantum Information **11**, 51 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2409.03744) · [npj QI](https://doi.org/10.1038/s41534-025-01003-2)
> **Tags:** #paper #QSP #QSVT #randomization #Hamiltonian-simulation #channel-approximation

---

## The computational problem

Given a block-encoding of operator $A$ and a target function $F: [-1,1] \to \mathbb{C}$, implement a quantum channel that approximates $\mathcal{F}_A(\rho) = F(A)\rho F(A)^\dagger$ (or the block-encoded version thereof) to diamond-norm error $\epsilon$, using as few queries to $A$ as possible.

In the deterministic [[QSVT Meta-Template|QSP/QSVT]] setting, the cost is $d$ queries where $d$ is the degree of a polynomial approximating $F$ to pointwise error $\epsilon$. For smooth functions like $e^{ix}$ or $(x+\kappa)^{-1}$, this gives $d = O(\log(1/\epsilon))$. The question: can we do better?

---

## What the paper does

Yes — by roughly a factor of 2, asymptotically. The paper introduces **Stochastic QSP**: instead of one deterministic QSP circuit of degree $d$, sample a random circuit from a strategically constructed ensemble of lower-degree polynomials. The [[Mixing Lemma for Block-Encodings|Hastings-Campbell mixing lemma]], extended to block-encodings, guarantees that the resulting channel has diamond-norm error $O(\epsilon)$ even though each individual circuit only achieves error $O(\sqrt{\epsilon})$. This quadratic error suppression means the ensemble needs average degree only $d/2 + O(1)$.

Because the standard cost driver across the [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|QSP/QSVT family]] is the polynomial degree (which enters as $\log(1/\epsilon)$ for precision-limited algorithms), this is a genuine factor-of-2 improvement in the precision-dependent part of query complexity across a wide range of algorithms: [[Hamiltonian simulation]], imaginary time evolution, phase estimation, ground state preparation, matrix inversion.

**Assessment:** This is a clean, general-purpose improvement — not a one-problem trick. The construction is elegant, the proof is short, and the improvement applies broadly. The main limitation is that you get a quantum channel (probabilistic mixture of unitaries) rather than a single coherent unitary, which precludes use as a coherent subroutine inside amplitude amplification. But in most practical measurement scenarios, a channel is exactly what you want.

---

## The algorithm: Stochastic QSP construction

The key insight: Chebyshev coefficients of smooth functions decay exponentially, so high-degree terms contribute little. Sample from a distribution that weights lower-degree polynomials heavily — and use the mixing lemma to turn the average-channel approximation into a diamond-norm guarantee.

**Step-by-step construction (Theorem 3):**

1. **Start with the deterministic approximation.** Let $P(x) = \sum_{n=0}^{d} c_n T_n(x)$ be the degree-$d$ Chebyshev truncation of $F(x)$, achieving $\|P - F\|_\infty \le \epsilon$. The coefficients satisfy $|c_n| \le C e^{-qn}$ for $n \ge d/2$.

2. **Partition into blocks.** Divide the Chebyshev indices $\{0, 1, \ldots, d\}$ into blocks $B_j$ by degree range (roughly geometric partitioning).

3. **Construct block polynomials.** For each block $B_j$, define a polynomial $P_j(x) = \sum_{n \in B_j} c_n T_n(x)$ (scaled so it has max degree equal to the block's upper index).

4. **Choose probabilities.** Set $p_j \propto \|P_j - P_{j-1}\|_\infty$ (roughly proportional to how much each block contributes). The exponential decay of Chebyshev coefficients means higher-degree blocks get exponentially smaller probability.

5. **Individual error.** Each $P_j$ satisfies $|P_j(x) - F(x)| \le 2\sqrt{\epsilon}$ — worse than the deterministic bound, but $O(\sqrt{\epsilon})$.

6. **Average error.** $\sum_j p_j (P_j(x) - F(x)) \le \epsilon$ — the average polynomial matches $F$ to $\epsilon$.

7. **Apply the [[Mixing Lemma for Block-Encodings]].** Each block polynomial implements a QSP circuit. The mixing lemma tells us that if individual spectral error is $\le a = 2\sqrt{\epsilon}$ and average error is $\le b = \epsilon$, the channel satisfies:
$$\|\Lambda_A - \mathcal{F}_A\|_\diamond \le a^2 + 2b = 4\epsilon + 2\epsilon = 6\epsilon$$

8. **Average degree.** Because $p_j$ decays exponentially with block index and block degrees increase geometrically, the average degree satisfies:
$$d_{\mathrm{avg}} \le \frac{d}{2} + O(1)$$

The $O(1)$ term depends on $C$ (coefficient amplitude bound) and $q$ (decay rate). Concretely, the cost reduction factor is:
$$\frac{2 d_{\mathrm{avg}}}{d} \lesssim 1 + \frac{\log C}{qd} \xrightarrow{d \to \infty} \frac{1}{2}$$

---

## Key results

### Theorem 3 — Stochastic QSP (main result)

Let $F: [-1,1] \to \mathbb{C}$ with $\|F\|_\infty \le 1$ have Chebyshev expansion $F(x) = \sum_n c_n T_n(x)$ with $|c_n| \le Ce^{-qn}$ for $n \ge d/2$. Let $P(x) = \sum_{n=0}^d c_n T_n(x)$ achieve $\|P - F\|_\infty \le \epsilon$.

Then there exists an ensemble $\{(P_j, p_j)\}$ such that:
$$|P_j(x) - F(x)| \le 2\sqrt{\epsilon} \quad \text{(individual error)}$$
$$\sum_j p_j (P_j(x) - F(x)) \le \epsilon \quad \text{(mean error)}$$
$$d_{\mathrm{avg}} := \sum_j p_j \deg(P_j) \le \frac{d}{2} + O(1) \quad \text{(average degree)}$$
$$\|\Lambda_A - \mathcal{F}_A\|_\diamond \le 6\epsilon \quad \text{(channel diamond-norm error)}$$

### Lemma 1 — Hastings-Campbell Mixing Lemma

If unitaries $\{U_j\}$ with probabilities $\{p_j\}$ satisfy:
- $\|U_j - V\| \le a$ (spectral norm, each individual)
- $\|\sum_j p_j U_j - V\| \le b$ (weighted average)

Then the randomized channel $\Lambda(\rho) = \sum_j p_j U_j \rho U_j^\dagger$ satisfies:
$$\|\Lambda - \mathcal{V}\|_\diamond \le a^2 + 2b$$

The quadratic error suppression $a \to a^2$ is the key: it lets individual error $O(\sqrt{\epsilon})$ combine into channel error $O(\epsilon)$.

### Lemma 2 — [[Mixing Lemma for Block-Encodings]] (key extension)

Extends Lemma 1 from bare unitaries to block-encoded operators. QSP works via block-encodings — the signal channel acts in a subspace — so the original Hastings-Campbell lemma doesn't apply directly. Lemma 2 proves the analogous bound for block-encoded channels, enabling Theorem 3.

### Corollary 4 — Parity preservation

The ensemble can be chosen such that each $P_j$ has the same definite parity as $F$. QSP requires definite-parity polynomials; this corollary ensures parity is not lost by the randomization.

### Corollary 5 — Works beyond Chebyshev

The construction extends to Taylor series and other bounded bases (not just Chebyshev), provided the coefficients have appropriate decay.

### Corollary 6 — Full QSP unitary approximated

The full QSP unitary (not just the block-encoded operator) is approximated to $O(\epsilon)$ in channel error. This means the result applies to the complete circuit, not just the sub-block.

---

## Application complexities

| Algorithm | Deterministic QSP query complexity | Stochastic QSP query complexity | Improvement |
|---|---|---|---|
| Real-time simulation $e^{-iHt}$ | $O\!\left(\alpha t + \frac{\log(1/\epsilon)}{\log\log(1/\epsilon)}\right)$ | $O\!\left(\alpha t + \frac{\log(1/\epsilon)}{2\log\log(1/\epsilon)}\right)$ | Factor $\approx 2$ on log term |
| Imaginary time / thermal $e^{-\beta H}$ | $O\!\left(\sqrt{\beta\alpha} \log(1/\epsilon)\right)$ | $O\!\left(\sqrt{\beta\alpha} \cdot \tfrac{1}{2}\log(1/\epsilon)\right)$ | Factor 2 on log |
| Phase estimation | $O\!\left(\alpha t \cdot \log(1/\epsilon)\right)$ | $O\!\left(\alpha t \cdot \tfrac{1}{2}\log(1/\epsilon)\right)$ | Factor 2 on log |
| Ground state preparation | $O\!\left(\Delta^{-1} \log(1/\epsilon)\right)$ | $O\!\left(\Delta^{-1} \cdot \tfrac{1}{2}\log(1/\epsilon)\right)$ | Factor 2 on log |
| Matrix inversion $A^{-1}$ | $O\!\left(\kappa \log(1/\epsilon)\right)$ | $O\!\left(\kappa \cdot \tfrac{1}{2}\log(1/\epsilon)\right)$ | Factor 2 on log |

The factor-of-2 improvement applies to the $\log(1/\epsilon)$ term specifically. When the problem-dependent prefactor ($\alpha t$, $\kappa$, $\Delta^{-1}$) dominates, the relative improvement shrinks. For precision-dominated regimes (high accuracy requirements), the improvement is significant.

---

## Comparison with prior work

### Stochastic QSP vs. deterministic QSP approaches

| Method | Circuit type | Output type | Precision term | Notes |
|---|---|---|---|---|
| Deterministic QSP [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|(Low-Chuang 2016)]] | Single circuit, degree $d$ | Unitary (coherent) | $O(\log(1/\epsilon))$ | Coherent, composable |
| Fully-coherent QSP [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|(Martyn et al. 2023)]] | Single circuit, no OAA | Unitary (coherent) | $\Theta(\log(1/\epsilon) + \log(1/\delta))$ | Bypasses parity; coherent concatenation |
| **Stochastic QSP (this paper)** | Ensemble, avg. degree $d/2$ | Channel (incoherent) | $O(\tfrac{1}{2}\log(1/\epsilon))$ | Factor-2 savings; not coherent |

The two Martyn lines are **complementary**: Martyn 2023 optimizes for coherence and unitary output; this paper optimizes for query count at the cost of coherence. Different tools for different situations.

### Stochastic QSP vs. other randomized simulation approaches

| Method | Paper | Randomization level | Output | Precision scaling |
|---|---|---|---|---|
| qDRIFT | [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell 2018]] | Hamiltonian term selection | Channel | $O((\lambda t)^2/\epsilon)$ — no log! |
| Randomized [[product formula]]s | [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu 2019]] | Trotter ordering | Channel | Improved Trotter error constants |
| Randomized MPF | [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann et al. 2022]] | Multi-[[product formula]] sampling | Observable estimation | Exponential depth reduction, $O(\Xi^4/\epsilon^2)$ shots |
| qFLO | [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes|Watson 2025]] | Richardson extrapolation of qDRIFT | Observable estimation | $O((\lambda T)^2 \log(1/\epsilon))$ depth |
| **Stochastic QSP** | **This paper** | **QSP polynomial ensemble** | **Channel** | $O(\tfrac{1}{2}\log(1/\epsilon))$ |

**Key differentiator:** Previous randomized methods operate at the level of Hamiltonian decomposition (which terms to sample, in which order). Stochastic QSP operates at the QSP/polynomial level — it's randomizing which approximating polynomial to use, not which Hamiltonian terms. This is a genuinely new application point for randomization.

**Connection to Marika's work:** The randomization philosophy here — sample from an ensemble, use the mixing lemma to convert average-channel improvement into a diamond-norm guarantee — is exactly the philosophy behind [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|qDRIFT]] and [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|randomized multi-product formulas]]. The novelty here is applying that philosophy one level up: at the polynomial/QSP layer rather than the product-formula layer. The mathematical machinery (mixing lemma, diamond-norm channel averaging) is the same; what changes is the object being randomized.

---

## Limits / caveats

1. **Channel, not unitary.** The output is a quantum channel $\Lambda_A(\rho) = \sum_j p_j U_j \rho U_j^\dagger$ — a probabilistic mixture of unitaries. This is suitable for observable estimation and for algorithms that accept channel-level approximation, but **cannot** be used as a coherent subroutine inside amplitude amplification or other protocols requiring a single unitary. If you need coherent concatenation, use [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn et al. 2023]] instead.

2. **Asymptotic factor.** The factor-of-2 is asymptotic — it approaches $1/2$ as $d \to \infty$ (i.e., as $\log(1/\epsilon)$ grows). The exact ratio is $1 + \log(C)/(qd)$, so for moderate precision or functions with large $C$ or small $q$, the improvement is less than $2\times$.

3. **Constant overhead.** The $O(1)$ additive term in $d_{\mathrm{avg}} \le d/2 + O(1)$ depends on $C$ and $q$ (Chebyshev coefficient bound parameters). For some functions, these constants are non-trivial, reducing the effective gain.

4. **Classical sampling overhead.** Running the stochastic scheme requires: (a) classically computing the ensemble $\{(P_j, p_j)\}$, (b) sampling $j \sim p_j$ before each circuit run, (c) synthesizing QSP phases for the chosen $P_j$. Step (c) can be pre-computed offline. The classical overhead is efficient but not free.

5. **Controlled QSP.** Phase estimation requires controlled-QSP circuits (conditioned on an ancilla qubit). In the stochastic setting, one must condition the sampled polynomial on the ancilla state, which complicates implementation slightly. The paper addresses this (Controlled QSP section, Corollary 6) but the overhead is worth noting.

---

## Reusable ideas

1. **[[Stochastic QSP via Chebyshev Tail Truncation]]** — Partition a Chebyshev approximation into blocks of varying degree; sample blocks according to their coefficient weight. Average degree halves; mixing lemma closes the gap from $O(\sqrt{\epsilon})$ individual error to $O(\epsilon)$ channel error.

2. **[[Mixing Lemma for Block-Encodings]]** — The Hastings-Campbell mixing lemma, extended from unitaries to block-encoded operators. The key enabling result: channel averaging works even when circuits act in a subspace defined by a projector.

3. **[[Diamond-Norm Channel Averaging for Random Compilers]]** — The general principle that individual circuits with spectral error $a$ combine via the mixing lemma to give channel diamond-norm error $\le a^2 + 2b$ (where $b$ is average error). This trick card already covers the unitary version; the block-encoding version is the new contribution of this paper.

---

## References within this paper

Key citations the paper builds on:

- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn-Rossi-Tan-Chuang (2021)]] — QSP/QSVT framework; this paper is a direct efficiency improvement on that family.
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2016–2017)]] — original optimal QSP [[Hamiltonian simulation]], the baseline being improved.
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization and block-encoding framework.
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT meta-framework; the broader context.
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn-Liu-Chin-Chuang (2023)]] — the complementary fully-coherent approach by the same first author.
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2018)]] — qDRIFT; the Hastings-Campbell mixing lemma originates here.
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — randomized [[product formula]]s; related randomization philosophy.
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]] — randomized MPF; same mixing lemma, applied to multi-[[product formula]]s.
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes|Watson (2025)]] — qFLO; another randomized approach in the same spirit.
- Hastings and Campbell (2019) — original Hastings-Campbell mixing lemma for unitaries. No vault note yet.

---

## Cross-links

### Paper notes
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]] — same first author; this paper improves the QSP/QSVT framework reviewed there
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — same first author; the coherent vs stochastic contrast is the central comparison
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — earlier mixing-lemma-based randomization at the product-formula level; doubles error order rather than halving query degree
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
- [[Paper Index]]

### Trick cards
- [[Stochastic QSP via Chebyshev Tail Truncation]]
- [[Mixing Lemma for Block-Encodings]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[QSVT Meta-Template]]
- [[Block-Encoding Composition Algebra]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[Randomized Multi-Product Sampling]]
- [[Fixed-Angle Randomized Product Formula]]
