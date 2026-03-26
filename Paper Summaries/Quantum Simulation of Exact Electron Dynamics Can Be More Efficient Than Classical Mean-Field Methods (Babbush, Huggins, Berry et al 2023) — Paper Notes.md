> **Source:** Ryan Babbush, William J. Huggins, Dominic W. Berry, Shu Fay Ung, Andrew Zhao, David R. Reichman, Hartmut Neven, Andrew D. Baczewski, Joonho Lee, *Quantum simulation of exact electron dynamics can be more efficient than classical mean-field methods*, arXiv:2301.01203, Nature Communications **14**, 4058 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2301.01203) · [Journal](https://doi.org/10.1038/s41467-023-39024-0)
> **Tags:** #quantum-simulation #electron-dynamics #first-quantized #mean-field #RT-TDHF #RT-TDDFT #quantum-advantage #classical-shadows #finite-temperature #plane-wave

---

## The computational problem

Simulate exact real-time electron dynamics for $\eta$ electrons in $N$ basis functions (plane-wave or real-space grid). The classical comparison point is **not** the most accurate correlated methods — it's the cheapest widely-used ones: real-time time-dependent Hartree-Fock (RT-TDHF) and real-time time-dependent density functional theory (RT-TDDFT). These solve

$$i\frac{\partial \mathbf{C}_\text{occ}(t)}{\partial t} = \mathbf{F}(t)\,\mathbf{C}_\text{occ}(t)$$

where $\mathbf{F}(t)$ is the time-dependent Fock matrix and $\mathbf{C}_\text{occ}(t)$ is the $N \times \eta$ matrix of occupied orbital coefficients. The goal: compare total operation counts for evolving to time $t$ at precision $\epsilon$, and for extracting observables (particularly the $k$-particle reduced density matrix).

This paper asks whether quantum algorithms can beat classical methods that are *already approximate*, not just methods that are exact-but-expensive. That's a different (and harder) bar than the usual quantum chemistry advantage question.

---

## What the paper does

Shows that [[First-Quantized Plane-Wave Chemistry Encoding|first-quantized]] quantum algorithms for dynamics beat classical mean-field methods (RT-TDHF, RT-TDDFT with hybrid functionals) in basis-set scaling by up to a quintic factor in $N$. The advantage comes from three sources:

1. **Exponential space compression:** $O(\eta \log N)$ qubits vs. $O(N\eta \log(1/\epsilon))$ classical bits
2. **Polynomial gate complexity reduction:** first-quantized Trotter achieves $(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t)(Nt/\epsilon)^{o(1)}$ vs. classical $(N^{4/3}\eta^{7/3}t + N^{5/3}\eta^{4/3}t)(Nt/\epsilon)^{o(1)}$
3. **$k$-RDM estimation with polylogarithmic $N$ dependence** via a first-quantized classical shadows procedure

The paper also introduces an improved [[Givens Rotation Slater Determinant Preparation|Slater determinant preparation]] algorithm for first quantization with $\widetilde{O}(N\eta)$ Toffoli cost, which is likely subdominant to the time-evolution cost for long simulations.

The strongest advantage appears at **finite temperature**, where classical mean-field must track $M \simeq N$ partially occupied orbitals, inflating cost to $\widetilde{O}(NM^2)$ per time step — while the quantum algorithm's cost is independent of temperature.

---

## The algorithm / construction

### Classical mean-field cost analysis

The Fock matrix update per time step costs $\widetilde{O}(N\eta^2)$ using occ-RI-K (Manzer et al. 2015) or ACE (Lin 2016). These exploit the fact that the effective rank of the Fock operator restricted to occupied orbitals scales as $O(\eta)$, not $O(N)$.

The spectral norm of the Fock operator is bounded as:

$$\max_{\mathbf{C}_\text{occ}} \|\mathbf{F}\| = O\!\left(\frac{\eta^{2/3}}{\delta} + \frac{1}{\delta^2}\right) = O\!\left(N^{1/3}\eta^{1/3} + \frac{N^{2/3}}{\eta^{2/3}}\right)$$

where $\delta = O((\eta/N)^{1/3})$ is the minimum grid spacing. The first term comes from the Coulomb operator, the second from kinetic energy.

Using an order-$k$ integrator, the number of time steps scales as $T^{1+1/k}/\epsilon^{1/k}$ where $T = \|\mathbf{F}\| \cdot t$. Taking $k \to \infty$: total classical cost is

$$\left(N^{4/3}\eta^{7/3}t + N^{5/3}\eta^{4/3}t\right)\left(\frac{Nt}{\epsilon}\right)^{o(1)}$$

At finite temperature with $M$ appreciably occupied orbitals (where $M \simeq N$ at high $T$), $\eta^2$ becomes $M^2$ in the per-step cost, giving:

$$\left(N^{4/3}M^2\eta^{1/3}t + \frac{N^{5/3}M^2 t}{\eta^{2/3}}\right)\left(\frac{Nt}{\epsilon}\right)^{o(1)}$$

### Quantum first-quantized Trotter (tightened bounds)

The paper tightens bounds on first-quantized Trotter simulation in a real-space grid using high-order Trotter formulas and recent commutator-based error bounds (Childs & Su 2019; Su et al. 2021; Low et al. 2022). Each Trotter step costs $\widetilde{O}(\eta^2)$ gates (switching between position and momentum representations via QFT on each particle register). The total gate complexity is:

$$\left(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t\right)\left(\frac{Nt}{\epsilon}\right)^{o(1)}$$

**This is the tightest reported Trotter bound for any first-quantized quantum chemistry simulation.** Compared to the classical mean-field cost, the $N$ exponent drops by 1 in each term — a full factor of $N$ improvement. When the kinetic-dominated term controls (i.e. $N > \Theta(\eta^4)$), this is a **quintic speedup in $N$** combined with a quadratic slowdown in $\eta$.

### Interaction picture and qubitized algorithms

The [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] algorithm from [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush et al. (2019)]] and the qubitized variant from [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] both achieve $\widetilde{O}(N^{1/3}\eta^{8/3}t)$. These have better $N$ dependence than Trotter but worse $\eta$ dependence, so they dominate when $N \gg \eta$.

### First-quantized classical shadows for $k$-RDM

The paper introduces a [[First-Quantized Classical Shadows for k-RDM|classical shadows procedure]] tailored to first quantization. The key idea: measuring the $k$-particle RDM requires estimating $O(N^{2k})$ matrix elements in second quantization, but in first quantization the cost is:

$$\widetilde{O}\!\left(\frac{k^k \eta^k L \,\mathcal{C}_\text{samp}}{\epsilon^2}\right)$$

where $L$ is the number of time points and $\mathcal{C}_\text{samp}$ is the cost of preparing a single sample of $|\psi(t)\rangle$. The number of samples is **polylogarithmic in $N$** — the basis size drops out of the measurement cost entirely.

The procedure works by:
1. Sampling a random $k$-subset $S$ of the $\eta$ particle registers
2. Applying a random Clifford on the $k \log N$ qubits of those registers
3. Measuring in the computational basis
4. Reconstructing all $\binom{N}{k}^2$ entries of the $k$-RDM from $O(k^k \eta^k / \epsilon^2)$ samples

This is based on the observation that first-quantized states have particle-permutation symmetry, which concentrates the shadow estimator.

### Slater determinant preparation in first quantization

New algorithm: convert a Slater determinant from second quantization (where it's preparable via [[Givens Rotation Slater Determinant Preparation|Givens rotations]]) to first quantization on-the-fly. The trick is that the Givens rotation network produces qubits sequentially — once a qubit is "done" (no more rotations needed), convert it from occupation-number to particle-register format. This needs only $O(\eta)$ ancilla qubits and costs $\widetilde{O}(N\eta)$ Toffolis total.

This is a one-time cost, not multiplied by the time-evolution duration. For long simulations it's negligible.

---

## Key results

### Zero-temperature dynamics (sampling output)

| Algorithm | Space | Gate complexity |
|---|---|---|
| Classical mean-field (occ-RI-K/ACE) | $O(N\eta \log(1/\epsilon))$ | $(N^{4/3}\eta^{7/3}t + N^{5/3}\eta^{4/3}t)(Nt/\epsilon)^{o(1)}$ |
| 2nd-quant. Trotter (grid) | $O(N \log N)$ | $(N^{4/3}\eta^{1/3}t + N^{5/3}t/\eta^{2/3})(Nt/\epsilon)^{o(1)}$ |
| **1st-quant. Trotter (grid) [this work]** | $O(\eta \log N)$ | $(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t)(Nt/\epsilon)^{o(1)}$ |
| 1st-quant. interaction picture | $O(\eta \log N)$ | $\widetilde{O}(N^{1/3}\eta^{8/3}t)$ |
| 1st-quant. qubitized (Su et al.) | $O(\eta \log N)$ | $\widetilde{O}(N^{1/3}\eta^{8/3}t)$ |

### Speedup summary

- **$N < \Theta(\eta^4)$ (Coulomb-dominated):** quantum Trotter gives $N^1$ factor improvement over classical mean-field
- **$N > \Theta(\eta^4)$ (kinetic-dominated):** quantum Trotter gives $N^1$ improvement in the dominant $N^{5/3}$ term → effectively quintic speedup in $N$, quadratic slowdown in $\eta$
- **Overall in system size $\Sigma = N\eta$:** up to $\Sigma^7$ speedup for the interaction picture when $N = \Theta(\eta^\alpha)$ with $\alpha > 4$
- **Finite temperature:** speedup amplified because classical cost scales as $M^2$ with $M \simeq N$ at high temperature, while quantum cost is temperature-independent

### Observable estimation

- Gradient-based measurement ([[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins et al. 2022]]): $\widetilde{O}(\sqrt{L}\,\mathcal{C}_\text{samp}\,\lambda/\epsilon)$ for a block-encodable observable with norm $\lambda$
- First-quantized shadows: $\widetilde{O}(k^k\eta^k L\,\mathcal{C}_\text{samp}/\epsilon^2)$ for the full $k$-RDM

---

## Comparison with prior work

| Paper | Year | What it compared against | Speedup type |
|---|---|---|---|
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush et al. (2019)]] | 2019 | Best exact quantum algorithms | Sublinear in $N$ |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su et al. (2021)]] | 2021 | Best exact quantum algorithms | Constant-factor compiled |
| [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes\|Lee et al. (2022)]] | 2022 | Best classical correlated methods | No EQA evidence for ground states |
| **This paper** | 2023 | **Cheapest classical methods (mean-field)** | Polynomial speedup even over approximate classical |

This paper flips the usual comparison. Prior work always asked "can quantum beat exact classical?" and typically found the answer unclear for ground states. This paper asks "can quantum beat *cheap* classical?" for dynamics, and the answer is a clear yes in scaling.

---

## Limits / caveats

1. **Measurement overhead kills some of the advantage.** Extracting the full $k$-RDM costs $O(1/\epsilon^2)$ shots, and for the 1-RDM all $N^2$ elements must be estimated. The shadows procedure is polylogarithmic in $N$ for sample count, but the classical post-processing to reconstruct $O(N^2)$ matrix elements is $O(N^2)$. So the end-to-end comparison is more nuanced than just the gate complexity ratio.

2. **The comparison assumes the best classical mean-field algorithms (occ-RI-K/ACE) but not linear-scaling methods.** Linear-scaling DFT achieves $O(N)$ per step using density matrix sparsity, but only works for gapped systems, only kicks in at large system sizes, and breaks down at finite temperature and for excited states. The paper argues these limitations make linear-scaling methods not the right comparison point, and I think that's fair — but worth noting.

3. **Second-quantized algorithms have the same $N$ scaling as classical mean-field.** The polynomial advantage in $N$ is specific to first quantization. Second-quantized Trotter (Low et al. 2022) matches classical mean-field in $N$ dependence and differs only in $\eta$.

4. **Practical crossover unknown.** The asymptotic analysis tells us the quantum algorithm wins for large enough systems, but no constant-factor compilation for the dynamics use case is provided (unlike Su et al. 2021 which compiled the phase estimation case). The gap between asymptotic advantage and practical advantage could be wide.

5. **Finite temperature advantage assumes the density matrix approach classical.** If classical finite-$T$ simulations also use trajectory sampling (sampling individual Slater determinants from the thermal distribution), the per-trajectory classical cost stays at $\widetilde{O}(N\eta^2)$ but picks up $O(1/\epsilon^2)$ statistical overhead — which partially closes the gap with quantum.

6. **Correlated classical dynamics methods exist** (e.g., RT-CCSD, DMRG-based) that are more expensive than mean-field but also more accurate. This paper's speedup over mean-field automatically implies speedup over these methods, since they scale at least as badly. But the relevant practical question is whether quantum beats the cheapest method that's *accurate enough for the application*, and that's system-dependent.

---

## My assessment

This paper shifts the narrative about quantum advantage in chemistry in an important direction. The standard framing — "quantum computers for strongly correlated ground states" — has been under pressure since [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee et al. (2022)]] found no evidence for exponential advantage. This paper sidesteps that issue entirely by targeting *dynamics*, where even approximate classical methods (mean-field) have worse scaling than exact quantum algorithms.

The conceptual insight is clean: first-quantized encoding compresses $N$ orbital coefficients into $\log N$ qubits per electron, and there's no classical analogue of this compression for dynamics (mean-field methods still need to store the full $N \times \eta$ coefficient matrix). This is a different source of advantage than the usual "exponential Hilbert space" argument — it's about efficient encoding of a distribution over basis functions, not of entanglement.

The finite-temperature direction is particularly promising. Classical mean-field cost blows up as $M^2 \simeq N^2$ for the high-$T$ density matrix, while the quantum algorithm is temperature-agnostic. This makes warm dense matter (WDM) simulations an appealing target for early quantum advantage — and the paper explicitly suggests this.

That said, the practical impact is hard to assess without constant-factor resource estimates. The asymptotic $N^5$ speedup could easily shrink to $N^2$ or less after accounting for error correction overhead, circuit compilation, and measurement costs. The paper is honest about this gap.

---

## Reusable ideas

1. [[First-Quantized Classical Shadows for k-RDM]] — Measure the $k$-RDM with sample complexity polylogarithmic in basis size $N$ by exploiting particle-permutation symmetry in first quantization
2. [[Second-to-First Quantization State Conversion]] — Convert a Slater determinant from second-quantized Givens rotation preparation to first-quantized registers on-the-fly, using only $O(\eta)$ ancilla qubits and $\widetilde{O}(N\eta)$ Toffolis
3. [[Classical-Quantum Runtime Crossover Analysis]] — already exists in the vault; this paper extends the methodology to dynamics rather than ground states

---

## References within this paper

- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush et al. (2019)]]: the [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] algorithm and [[First-Quantized Plane-Wave Chemistry Encoding]] that this paper builds on
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]]: constant-factor compilation of first-quantized algorithms; Appendix K grid algorithms used here
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins et al. (2022)]]: gradient-based observable estimation used for energy and arbitrary observables
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee et al. (2022)]]: context — this paper proposes dynamics as an alternative avenue for quantum advantage since ground-state EQA is unresolved
- Childs & Su (2019): tight Trotter error bounds used to tighten the first-quantized Trotter complexity
- Low, Su, Tong, Tran (2022): second-quantized Trotter grid algorithm providing the comparison point
- Manzer, Horn, Mardirossian, Head-Gordon (2015): occ-RI-K method for efficient classical Fock matrix application
- Lin (2016): Adaptively Compressed Exchange (ACE), alternative to occ-RI-K with same scaling
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan et al. (2018)]]: [[Givens Rotation Slater Determinant Preparation]] used as starting point for the new first-quantized state preparation

---

## Cross-links

### Paper notes
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — direct predecessor; provides the interaction picture algorithm and first-quantized encoding
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — compiled version of the algorithms; Appendix K grid basis algorithms cited
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — motivational context; ground-state EQA is unresolved
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — gradient measurement algorithm used for observable estimation
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Givens rotation framework for state preparation
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — earlier first-quantized simulation (CI matrix approach)
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — earlier real-space grid Trotter analysis
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — second-quantized plane-wave comparison point
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Same year, overlapping authors (Babbush, Berry, Wiebe); that paper addresses classical dynamics with exponential speedup, this one addresses quantum dynamics with polynomial-to-quintic speedup
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Applies the asymptotic advantage established here to a concrete dynamics problem (ICF stopping power); provides end-to-end constant-factor resource estimates using both QSP and 8th-order Trotter
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — matchgate shadows are the second-quantized measurement complement to this paper's first-quantized classical shadows for $k$-RDMs; the two approaches handle different encodings of the same fermionic observables
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — proves that two-copy measurements eliminate the $n^k$ sample-count penalty that single-copy matchgate shadows have; directly relevant to the $k$-RDM estimation costs here
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] — Resolves the $O(\eta^2)$ Coulomb bottleneck that limits this paper's product-formula approach; reduces per-step cost to $\widetilde{O}(\eta)$ via quantum FMM

### Trick cards
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the qubit encoding this paper builds on
- [[Interaction Picture Simulation (Kinetic Frame)]] — the algorithm framework for the best asymptotic results
- [[Givens Rotation Slater Determinant Preparation]] — basis of the state preparation method
- [[First-Quantized Classical Shadows for k-RDM]] — new trick introduced in this paper
- [[Second-to-First Quantization State Conversion]] — new trick introduced in this paper
- [[Classical-Quantum Runtime Crossover Analysis]] — methodology extended to dynamics
- [[Kinetic Energy as Bit-Product LCU]] — used in the Su et al. qubitized variant
- [[Incremental Kinetic Energy Register]] — used in the interaction picture variant
