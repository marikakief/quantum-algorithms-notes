> **Source:** Andrew M. Childs, Aaron Ostrander, and Yuan Su, *Faster quantum simulation by randomization*, Quantum **3**, 182 (2019); arXiv:1805.08385
> **Links:** [Quantum](https://quantum-journal.org/papers/q-2019-09-02-182/) · [arXiv](https://arxiv.org/abs/1805.08385)
> **Tags:** #hamiltonian-simulation #product-formulas #randomization #trotter

---

## What the paper does

Shows that randomly permuting the ordering of Hamiltonian terms in a [[Product Formulas|product formula]] gives provably stronger error bounds than any fixed ordering. The improvement is sometimes asymptotically better than bounds that exploit commutation structure, despite using less information about the Hamiltonian. This is a different randomization strategy from [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|qDRIFT]] — here you shuffle a full product formula rather than sampling individual terms.

---

## The algorithm

For $H = \sum_{j=1}^L H_j$, the $(2k)$-th order Suzuki formula $S_{2k}(\lambda)$ uses a fixed ordering of the $L$ summands. This paper instead draws a fresh random permutation $\sigma \in \text{Sym}(L)$ for each time segment, defining:

$$S_{2k}^\sigma(\lambda) := \text{the Suzuki formula with summands reordered by } \sigma$$

To simulate for time $t$ with $r$ segments, the algorithm:
1. For each segment $i = 1, \ldots, r$: draw $\sigma_i$ uniformly from $\text{Sym}(L)$
2. Apply $S_{2k}^{\sigma_r}(-it/r) \cdots S_{2k}^{\sigma_1}(-it/r)$

This implements a random unitary channel whose expectation approximates $e^{-iHt}$.

For the first-order case, a simpler variant suffices: randomly choose between the forward product $S_1(-it/r)$ and the reverse $S_1^{\text{rev}}(-it/r)$, using just 1 bit of randomness per segment.

---

## Why randomization helps: the cancellation mechanism

The Taylor expansion of a product formula's error contains **nondegenerate terms** (all summand indices distinct) and **degenerate terms** (some indices repeated). For terms of order $\leq L$:
- Nondegenerate terms dominate the error — they contribute $\Theta(L^s)$ at order $s$
- Degenerate terms contribute only $O(L^{s-1})$

**Randomization Lemma (Lemma 2):** The $s$-th order nondegenerate term of the average evolution $\frac{1}{L!}\sum_\sigma S_{2k}^\sigma(\lambda)$ equals

$$\frac{[(q_1 + \cdots + q_\kappa)\lambda]^s}{s!} \sum_{\substack{m_1, \ldots, m_s \\ \text{pairwise different}}} H_{m_1} \cdots H_{m_s}$$

which is exactly the $s$-th order nondegenerate term of $V(\lambda) = e^{\lambda H}$. So **all nondegenerate error terms cancel completely** in the average.

This leaves only the degenerate terms, which scale as $O(L^{s-1})$ rather than $O(L^s)$ — saving one power of $L$ at each order.

---

## Key results

**Theorem 1 (First-order randomized bound):**

$$\left\|\mathcal{V}(-it) - \frac{1}{2^r}\left(\mathcal{S}_1(-it/r) + \mathcal{S}_1^{\text{rev}}(-it/r)\right)^r\right\|_\diamond \leq \frac{(\Lambda|t|L)^4}{r^3} e^{2\Lambda|t|L/r} + \frac{2(\Lambda|t|L)^3}{3r^2} e^{\Lambda|t|L/r}$$

The randomized first-order formula is effectively second-order.

**Theorem 2 (Higher-order randomized bound):**

$$\left\|\mathcal{V}(-it) - \left(\frac{1}{L!}\sum_\sigma \mathcal{S}_{2k}^\sigma(-it/r)\right)^r\right\|_\diamond \leq O\!\left(\frac{(tL)^{4k+2}}{r^{4k+1}} + \frac{t^{2k+1}L^{2k}}{r^{2k}}\right)$$

The error has two competing terms: the first comes from squaring the individual-operation error (via the mixing lemma), the second from the degenerate terms in the average.

**Gate complexity comparison (assuming $\Lambda = O(1)$):**

| Method | Gate complexity |
|---|---|
| Deterministic $(2k)$-th order | $O\!\left(tL^2 \left(\frac{tL}{\varepsilon}\right)^{1/(2k)}\right)$ |
| **Randomized $(2k)$-th order** | $\max\!\left\{O\!\left(tL^2 \left(\frac{tL}{\varepsilon}\right)^{1/(4k+1)}\right),\, O\!\left(tL^2 \left(\frac{t}{\varepsilon}\right)^{1/(2k)}\right)\right\}$ |
| Commutator bound (1D Heisenberg) | $\max\!\left\{O\!\left(tL^2 \left(\frac{tL}{\varepsilon}\right)^{1/(2k+1)}\right),\, O\!\left(tL^2 \left(\frac{t}{\varepsilon}\right)^{1/(2k)}\right)\right\}$ |

The randomized formula always improves the $L$-dependence. When $t = o(L^{2k})$, it also beats the commutator bound despite using less structural information.

---

## The mixing lemma (key technical tool)

**Lemma 1 (Campbell-Hastings):** Given a target unitary $V$ and random unitaries $\{U_j\}$ with probabilities $\{p_j\}$:
- If $\|U_j - V\| \leq a$ for all $j$ (individual error)
- If $\|\sum_j p_j U_j - V\| \leq b$ (average error)

Then $\|\sum_j p_j \mathcal{U}_j - \mathcal{V}\|_\diamond \leq a^2 + 2b$.

The diamond-norm error of the channel depends **linearly** on the average-unitary error but only **quadratically** on the individual-unitary error. This is why cancellation in the average (via randomization) translates to reduced channel error.

---

## Comparison: randomization strategies for simulation

| Method | What's randomized | Basic unit | $L$-dependence | $\varepsilon$-dependence |
|---|---|---|---|---|
| [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes\|qDRIFT]] | Which term to apply | Single exponential | Independent of $L$ | $O(1/\varepsilon)$ |
| **This paper** | Term ordering | Full shuffled [[Product Formulas\|product formula]] | Reduced vs deterministic | $O(1/\varepsilon^{1/(2k)})$ or better |
| [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes\|Cho-Berry-Hsieh (2022)]] | Correction unitaries | Half-step + random correction | Similar to this paper | Doubles the order to $4k+2$ |

---

## Limits / caveats

- **Still a product-formula method.** Can't compete with [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]]/[[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP]] for high-precision black-box simulation.
- **Random circuit.** Each run produces a different circuit, which complicates verification and makes the method less suitable when deterministic circuits are needed.
- **$\Theta(L \log L)$ bits of randomness per segment** for the higher-order case (a full random permutation). Not a practical concern, but worth noting.
- **Bounds may still be loose.** Numerical evidence suggests both deterministic and randomized formulas perform much better than rigorous bounds indicate. Whether the *gap* between randomized and deterministic performance persists in practice is partially answered by Section 6's numerics: yes, but the advantage is modest for small systems.
- **Does not exploit locality.** The randomization improvement is structure-agnostic. For lattice Hamiltonians, the [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes|Childs-Su local error analysis]] gets better scaling by directly exploiting the geometry.

---

## Reusable ideas

1. [[Permutation Averaging for Product-Formula Error Cancellation]] — averaging over all orderings of Hamiltonian terms cancels the dominant (nondegenerate) error terms, leaving only the smaller degenerate contributions
2. [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]] — the Campbell-Hastings mixing lemma converts a good average-case bound into a diamond-norm channel guarantee, with linear dependence on average error and quadratic on individual error
3. [[Nondegenerate-vs-Degenerate Taylor-Term Bookkeeping]] — classifying Taylor expansion terms by whether summand indices repeat; nondegenerate terms scale as $L^s$, degenerate as $L^{s-1}$

---

## References within this paper

- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — qDRIFT; this paper extends randomization to higher-order Suzuki formulas
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2021)]] — deterministic Trotter error theory (companion paper by overlapping authors)
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — Suzuki integrator analysis
- Campbell-Hastings (2017/2018) — the mixing lemma (arXiv:1811.08017, arXiv:1706.02176)
- Zhang (2010) — earlier randomized ordering for first-order formulas (trace-distance bound only)
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin et al. (2011)]] — Monte Carlo time-sampling for time-dependent Hamiltonians

---

## Cross-links

### Paper notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] — different randomization strategy: sample individual terms by weight
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — deterministic commutator-based error theory; this paper's randomized bounds sometimes beat those commutator bounds
- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]] — companion paper achieving near-optimal lattice simulation via local error analysis (different technique, complementary result)
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — builds directly on this work; doubles the error order from $2k+1$ to $4k+2$
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — combines multi-product formulas with randomized sampling
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry-based error cancellation (complementary to randomization)
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — earlier randomised [[Product Formulas]] for time-dependent Hamiltonians
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — Richardson extrapolation over qDRIFT step sizes
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — randomization at the QSP polynomial level rather than Hamiltonian term level

### Trick cards
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[Nondegenerate-vs-Degenerate Taylor-Term Bookkeeping]]
- [[Monte-Carlo Permutation Error Estimation for Product Formulas]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[Fixed-Angle Randomized Product Formula]]
- [[Randomized Correction to Double Product-Formula Order]]
