> **Source:** Paul K. Faehrmann, Mark Steudtner, Richard Kueng, Mária Kieferová, Jens Eisert, *Randomizing multi-[[Product Formulas]]s for [[Hamiltonian simulation]]*, Quantum **6**, 806 (2022)  
> **Links:** [arXiv:2101.07808](https://arxiv.org/abs/2101.07808) · [Quantum journal](https://doi.org/10.22331/q-2022-09-19-806)  
> **Tags:** #hamiltonian-simulation #multi-product #randomization #product-formulas

---

## What the paper does

Multi-[[Product Formulas]]s (MPFs) combine product-formula circuits at different step counts to cancel higher-order Trotter errors. The problem: doing this coherently requires implementing a linear combination of unitaries — ancilla registers, oblivious amplitude amplification, significant overhead. This paper cuts that overhead by replacing coherent LCU with randomized sampling. Instead of building the coherent superposition $\sum_q C_q V_q$, you sample term $q$ with probability proportional to $|C_q|$, run the corresponding product-formula circuit, and estimate observables classically. The cost is $O(\Xi^4/\varepsilon^2)$ shots, where $\Xi = \sum_q |C_q|$ is the resolution factor.

Two new MPF families are introduced — "matching" and "closed-form" — specifically designed to keep $\Xi$ small, which is the key to making the shot overhead tolerable.

---

## Background: multi-[[Product Formulas]]s

For $H = \sum_{j=1}^L H_j$, a $2\chi$-th order Suzuki formula $S_{2\chi}(t)^r$ has error that scales as $t^{2\chi+1}$ in the time step. A multi-[[Product Formulas]] takes a linear combination:
$$V_{\mathrm{MPF}} = \sum_{q=1}^K C_q \, S_{2\chi}\!\left(\frac{t}{\ell_q}\right)^{\!\ell_q}$$
choosing coefficients $C_q$ and step counts $\ell_q$ so that error terms cancel up to much higher order. The Childs-Wiebe approach picks coefficients via a Vandermonde system — the $C_q$ are solutions to a linear system that enforces cancellation up to order $2\chi K$.

**The deterministic implementation problem:** $V_{\mathrm{MPF}}$ is not a unitary — it is a *linear combination* of unitaries. To implement it coherently you need the full LCU machinery: PREPARE + SELECT oracles, $O(\log K)$ ancilla qubits, and oblivious amplitude amplification to boost the success probability. That's substantial overhead, particularly for near-term devices. The resolution factor $\Xi = \sum_q |C_q|$ shows up as the success probability $1/\Xi^2$ in the LCU postselection step.

---

## The randomized sampling protocol

**Algorithm 1 (informal):**

1. Compute $\Xi = \sum_q |C_q|$ and probabilities $p_q = |C_q|/\Xi$.
2. For each of $N$ repetitions:
   a. Sample index $q$ with probability $p_q$.
   b. Run circuit $V_q = S_{2\chi}(t/\ell_q)^{\ell_q}$ (a standard [[Product Formulas]] — no ancillas needed).
   c. Measure observable $O$ using the **Hadamard test**: a single ancilla qubit, a controlled-$V_q$ gate, and a measurement. This gives an unbiased estimator of $\mathrm{Re}\langle \psi | V_q^\dagger O V_q | \psi \rangle$.
3. Classical post-processing: multiply each sample by $\mathrm{sgn}(C_q) \cdot \Xi^2$ and average.

The estimator is unbiased for $\mathrm{tr}(O \, U(T) \rho \, U(T)^\dagger)$ up to the MPF approximation error.

**Why one ancilla qubit?** The Hadamard test extracts $\mathrm{Re}\langle \psi | V_q^\dagger O V_q | \psi \rangle$ using a single ancilla — far cheaper than the $O(\log K)$ ancilla register needed for coherent LCU. The ancilla just controls which circuit runs; it's measured and discarded each shot.

**Structural note:** This works for observable estimation only. You cannot reconstruct the full quantum state this way — the randomization averages over different unitaries, so the output is a classical mixture. Fine for most near-term applications; a genuine constraint nonetheless.

---

## Two new MPF families

### Matching MPF

Built multiplicatively: define $R$ layers, each layer a linear combination that corrects one more error order:
$$V_{\mathrm{match}} = \prod_{r=1}^{R} \left( \sum_{q} C_q^{(r)} \, S_{2\chi}\!\left(\frac{t}{\ell_q^{(r)}}\right)^{\!\ell_q^{(r)}} \right)$$

Each layer adds one more order of error cancellation. The **key property** of the multiplicative structure: the resolution factor satisfies
$$\Xi \leq \left(1 + \frac{2(g_\chi \Lambda T)^{2\chi+1}}{(2\chi+1)!}\right)^R$$
which is close to 1 when $\Lambda T$ is small (short-time or small Hamiltonian norm). For practical regimes where $\Lambda T < 1$, the sampling penalty $\Xi^4$ stays mild. This is the main advantage over Childs-Wiebe Vandermonde MPFs, which can have large $\Xi$ because the Vandermonde coefficients can be large in magnitude.

### Closed-form MPF

Uses Gauss-Legendre quadrature nodes $\{x_q\}$ and weights $\{w_q\}$ as the step counts and coefficients:
$$V_{\mathrm{cf}} = \sum_{q=1}^K w_q \, S_{2\chi}\!\left(\frac{t}{x_q}\right)^{x_q}$$

The advantage: fully explicit, no linear system to solve. The disadvantage: the Gauss-Legendre weights can be less optimized for small $\Xi$ than the matching construction. In practice the paper shows closed-form MPFs still outperform Vandermonde MPFs in resolution factor, just by a smaller margin than matching MPFs.

---

## The resolution factor $\Xi$

$\Xi = \sum_q |C_q|$ is the central quantity. It appears in three places:

1. **Shot count:** $N \geq 2\|O\|^2 \ln(2/\delta) \, \Xi^4 / \varepsilon^2$ repetitions needed (Theorem 2).
2. **LCU success probability (coherent case):** $1/\Xi^2$ — showing why large $\Xi$ kills the deterministic approach too.
3. **Error bound:** The approximation error $\|V_{\mathrm{MPF}} - U(T)/\Xi\| \leq \ldots$ is stated in terms of $\Xi$.

**Intuition:** $\Xi$ measures how "cancellative" the linear combination is. If all $C_q > 0$, there's no cancellation, $\Xi = \sum C_q = 1$ by normalization, and things are fine. If the formula requires large positive and negative coefficients that nearly cancel, $\Xi$ grows, and the sampling overhead blows up. The matching MPF design philosophy is precisely to minimize $\Xi$ by keeping the coefficient magnitudes small.

**The $\Xi^4$ penalty:** this is not a typo. Each shot gives an estimator of $\mathrm{tr}(O V_q \rho V_q^\dagger)$, and the classical combination involves multiplying by $\Xi^2$ per sample. When you square to get the variance, you get $\Xi^4$. This is unavoidable with importance sampling over signed linear combinations.

---

## Key results

**Theorem 1 (direct observable estimation):** For any state $\rho$, observable $O$, and unitary ensemble $\{V_q, p_q\}$ that samples exactly from $\sum_q C_q V_q \rho V_q^\dagger$, the estimator described above achieves additive error $\varepsilon$ with failure probability $\delta$ using
$$N \geq \frac{2\|O\|^2 \ln(2/\delta)}{\varepsilon^2}$$
repetitions. (This is the ideal case where the ensemble implements the MPF exactly.)

**Theorem 2 (approximate ensemble):** When the ensemble approximates $U(T)$ with resolution factor $\Xi$ — meaning $\|(1/\Xi)\sum_q C_q V_q - U(T)\| \leq \eta$ — the total error $\varepsilon$ is achievable with
$$N \geq \frac{2\|O\|^2 \ln(2/\delta) \, \Xi^4}{\varepsilon^2}$$
shots. The MPF approximation error $\eta$ contributes separately; choose circuit depth to make $\eta \leq \varepsilon/2$.

**Theorem 3 (Matching MPF error and $\Xi$ bounds):** For an $R$-layer matching MPF built on $2\chi$-th order Suzuki:
$$\left\|\Xi V - U(T)\right\| \leq \frac{(g_\chi \Lambda T)^{2\chi(R+1)+1}}{(2\chi(R+1)+1)!}$$
$$\Xi \leq \left(1 + \frac{2(g_\chi \Lambda T)^{2\chi+1}}{(2\chi+1)!}\right)^R$$
Here $g_\chi$ is a constant depending on the Suzuki order and $\Lambda = \sum_j \|H_j\|$. Error suppression is exponential in $R$ (the number of layers). For small $\Lambda T$, $\Xi$ is close to 1.

**Theorem 4 (Closed-form MPF):** Similar bounds with explicit Gauss-Legendre coefficients. Error scales as $(g_\chi \Lambda T)^{2\chi K + 1}$ for a $K$-term formula. The $\Xi$ bound is slightly less tight than for matching MPF but the formula is fully explicit.

---

## Comparison with prior work

| Method | Circuit depth | Ancilla qubits | Shot overhead | Precision scaling |
|---|---|---|---|---|
| Trotter-Suzuki $2\chi$-th order | $O(r \cdot L \cdot 5^\chi)$ | 0 | 1 | $O((LT)^{1+1/2\chi}/\varepsilon^{1/2\chi})$ |
| Deterministic MPF (Childs-Wiebe) | Similar to Trotter, + LCU overhead | $O(\log K)$ | 1 | Exponential in depth |
| [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes\|qDRIFT (Campbell 2019)]] | $O((\lambda T)^2/\varepsilon)$ | 0 | 1 | Linear in $1/\varepsilon$ (first-order) |
| This paper (randomized MPF) | Product-formula depth (tunable) | 1 (Hadamard test) | $O(\Xi^4/\varepsilon^2)$ | Exponential in circuit depth |
| [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes\|qFLO (Watson 2025)]] | $O((\lambda T)^2 \log(1/\varepsilon))$ | 0 | $O(1/\varepsilon^2)$ | Richardson over qDRIFT |

**vs deterministic MPF:** The randomized approach eliminates the ancilla register and OAA. Trade: you need $O(\Xi^4/\varepsilon^2)$ shots instead of one run.

**vs qDRIFT:** qDRIFT achieves first-order error cancellation. This paper gets exponential cancellation in circuit depth, at the cost of more complex sampling. For high precision and moderate $\Xi$, randomized MPF can win.

**vs Watson 2025 (qFLO):** Watson applies Richardson extrapolation (essentially incoherent multiproduct) to qDRIFT. The spirit is the same as this paper — both trade circuit depth for shots by combining multiple product-formula estimates. The difference is that Watson uses *classical* Richardson combination of qDRIFT estimates, while this paper samples from explicit MPF term distributions. For observable estimation, both approaches deliver comparable benefits; Watson's requires no Hadamard test (no controlled gates at all), while this paper's matching MPF gives more explicit control over $\Xi$.

---

## Numerical results

Tested on four models:
- **Nearest-neighbour Heisenberg** ($n$ qubits on a line)
- **Power-law Heisenberg** (interactions decaying as $r^{-\alpha}$)
- **Molecular hydrogen** (H₂ in minimal basis)
- **Sachdev-Ye-Kitaev (SYK) model**

The headline: matching and closed-form MPFs achieve substantially smaller $\Xi$ than Vandermonde-coefficient MPFs across all test cases. For example, at the simulation parameters tested, the Vandermonde MPF might have $\Xi \approx 5$–$10$, while the matching MPF achieves $\Xi \approx 1.05$–$1.5$. Since the shot count scales as $\Xi^4$, this is a $100\times$–$4000\times$ reduction in required shots.

Caveat: numerics are on small systems. How the comparison holds up at scale is left to future work.

---

## Limits / caveats

- **$\Xi^4$ shot scaling.** Even with small $\Xi$, the $1/\varepsilon^2$ shot requirement is unavoidable — this is the standard cost of classical Monte Carlo. For high-precision estimation, this can be expensive.
- **Observable estimation only.** The output is a classical expectation value, not a quantum state. Cannot be used for ground state preparation, QPE inputs, or any task requiring the actual evolved state.
- **Hadamard test requires controlled unitaries.** The controlled-$V_q$ gate may have non-trivial compilation cost. Watson 2025 avoids this entirely by not using a Hadamard test.
- **Numerics on small systems.** The $\Xi$ comparison is compelling but the test cases are small enough that asymptotic scaling may not have kicked in.
- **Not ancilla-free.** One ancilla qubit is still needed for the Hadamard test. Minor in practice but worth noting.

---

## Reusable ideas

- [[Randomized Multi-Product Sampling]] — the core trick: replace coherent LCU with importance sampling over signed linear combinations of product-formula circuits.
- [[Resolution Factor Minimization for Multi-Product Formulas]] — designing MPFs to minimize $\Xi = \sum|C_q|$; multiplicative layering (matching MPF) and Gauss-Legendre quadrature (closed-form MPF) as two concrete approaches.
- [[Importance Sampling over Hamiltonian Terms]] — broader context for importance-sampled [[Hamiltonian simulation]].

---

## References within this paper

Key references:

- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — qDRIFT: the direct precursor in randomized [[Hamiltonian simulation]]; this paper can be seen as a higher-order generalization
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — LCU origins; the multi-[[Product Formulas]] construction (Vandermonde MPF) is from here; also where $\Xi$ appears as the LCU normalization constant
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2019)]] — randomized product-formula ordering; the other major randomized simulation result; a complementary approach (randomize *ordering*, not *which term in a linear combination*)
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — Trotter error theory providing the foundation for MPF error analysis
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. (2015)]] — Taylor series + LCU, the standard LCU-based simulation method this paper offers an alternative to
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2016–2017)]] — QSP: the asymptotic standard; comparison upper bound
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT: broader framework context
- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes|Low, Kliuchnikov & Wiebe (2019)]] — well-conditioned multiproduct formulas; solves the conditioning problem ($\|\vec{a}\|_1$ from $e^{\Omega(m)}$ to $O(\log m)$) that makes deterministic and randomised MPFs practical; the direct precursor to this paper

---

## Cross-links

**Paper notes:**
- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]] — the conditioning result that makes MPFs practical
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]]
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — early randomised [[Product Formulas]] approach (Monte Carlo time-sampling); different randomisation strategy from the MPF importance sampling here
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]

**Trick cards:**
- [[Randomized Multi-Product Sampling]]
- [[Resolution Factor Minimization for Multi-Product Formulas]]
- [[Importance Sampling over Hamiltonian Terms]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[Fixed-Angle Randomized Product Formula]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[Richardson Extrapolation over Randomized Step Sizes]]
- [[Channel Series Expansion via Variation-of-Parameters]]
- [[Chebyshev-Node Conditioning for Multiproduct Formulas]] — the core conditioning trick from LKW19
- [[Order-Optimized Multiproduct Simulation]] — order selection strategy from LKW19

**Related (alternative approaches to improving [[Product Formulas]]s):**
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — deterministic correctors rather than randomised sampling; achieves factor-$\alpha$ error improvement for perturbed systems while staying fully ancilla-free
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — concurrent work (2022); uses randomized corrections within a single product formula to double error order; shares the mixing-lemma analysis framework
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — finds high-order product formulas that drastically reduce constant-factor error; complementary to the randomised sampling approach here
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — foundational Suzuki product-formula theory that underpins the MPF error analysis used here
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — alternative strategy for reducing Trotter error via symmetry kicks; complementary to the multi-product approach here
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — Marika's earlier paper; pursues Dyson-LCU as an alternative to the product-formula paradigm this paper improves
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]] — Marika's earlier paper; uses LCU-based combination of unitaries, the same structural idea underlying the MPF approach
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — practical Trotter resource estimation for condensed-phase systems; a target application where MPF cost reductions from this paper would apply
