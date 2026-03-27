> **Source:** Pedro C. S. Costa, Dong An, Ryan Babbush, and Dominic W. Berry, *The discrete adiabatic quantum linear system solver has lower constant factors than the randomized adiabatic solver*, arXiv:2312.07690, Quantum **9**, 1887 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2312.07690) · [Quantum journal](https://quantum-journal.org/papers/q-2025-10-20-1887/)
> **Tags:** #QLSP #linear-systems #adiabatic #qubitization #quantum-walk #constant-factors #resource-estimation #numerical-benchmarks

---

## What the paper does

This is a follow-up to [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2022)]] that answers a practical question the original paper left open: how tight are the analytical constant factors? The answer: they're about **1,500× loose**. Through numerical simulation of the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk operator on random matrices, they find the actual constant is $\alpha \approx 1.56$ in $T = \alpha\kappa/\Delta$, compared to the analytical bound of $\alpha = 2305$.

The paper also benchmarks the randomized adiabatic method (Subaşı, Somma, Orsucci 2019; improved by Jennings et al. 2023) on the same instances and finds the discrete walk method is **~20× cheaper** in practice, directly contradicting the claim in Jennings et al. that the randomized approach is faster for certain condition numbers.

My assessment: this is not a new algorithm — it's a numerical validation paper. But it's an important one. The original Costa et al. result had constant factors so large ($\sim 5632\kappa/T$ for PD Hermitian, $\sim 15307\kappa/T$ for general $A$) that it was hard to take seriously for resource estimation. This paper makes the optimal QLSP solver viable for practical costing. It also settles a minor controversy about whether the randomized method could be competitive. (It can't.)

---

## The computational problem

Same as [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2022)]]: given a block encoding of $A$ with $\|A\| = 1$ and condition number $\kappa$, produce a state $\varepsilon$-close to $A^{-1}|b\rangle / \|A^{-1}|b\rangle\|$. Lower bound: $\Omega(\kappa \log(1/\varepsilon))$.

---

## Numerical methodology

### Quantum walk method

They simulate the full quantum circuit for the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk operator $W_T(s)$, not just the Hamiltonian evolution. Matrices are $4\times4$, $8\times8$, and $16\times16$ random matrices (both positive definite Hermitian and general non-Hermitian), with condition numbers $\kappa \in \{10, 20, 30, 40, 50\}$ and 100 independent samples per $\kappa$.

The test protocol: start with a small total step count $T$ and increment until $\||\tilde{x}\rangle - |x\rangle\| \leq \Delta = 0.2$. The schedule function uses $p = 1.4$ (not the analytically convenient $p = 3/2$ from the original paper — they optimize numerically).

Key numerical results (non-Hermitian, $8 \times 8$, fixed step count):

| $\kappa$ | Steps $T$ | RMS error |
|---|---|---|
| 10 | 64 | 0.188 |
| 20 | 140 | 0.202 |
| 30 | 220 | 0.204 |
| 40 | 304 | 0.203 |
| 50 | 392 | 0.199 |

At $\kappa = 50$: $\alpha = T\Delta/\kappa = 392 \times 0.2 / 50 \approx 1.57$. This is ~1,500× smaller than the analytical upper bound $\alpha = 2305$.

### Randomized method

They also test the randomized adiabatic method (Subaşı et al. 2019, with optimal probability distributions from Sanders et al. 2020), measuring total evolution time $\sum_j |t_j|$. Same matrix instances, same $\kappa$ values. For the randomized method, the complexity is in terms of evolution time, not walk steps — a distinction the paper emphasizes because converting evolution time to actual gate count adds further overhead (the randomized method requires continuous [[Hamiltonian simulation]]).

At $\kappa = 50$, non-Hermitian: the randomized method needs total time $\sim 7717$, compared to $\sim 392$ walk steps for the discrete method. That's a factor of ~20.

### Combined cost with [[Dolph-Chebyshev Eigenstate Filtering|filtering]]

After the adiabatic step achieves overlap $1-\delta$ (with $\delta = \Delta^2 - \Delta^4/4$), the [[Dolph-Chebyshev Eigenstate Filtering|Dolph-Chebyshev filter]] boosts precision to $\varepsilon$. The total cost is:

$$\frac{1}{2}\kappa\ln(2/\varepsilon_f)\left(\frac{1}{[1 - \delta(1-\varepsilon_f^2)]^2} + 1\right) + \frac{\alpha\kappa}{\Delta[1 - \delta(1-\varepsilon_f^2)]^2}$$

where $\varepsilon_f$ is the filter parameter. The optimal $\Delta$ (minimizing total cost) is around 0.3 for $\alpha = 1$, and around 0.15 for $\alpha = 0.2$ (PD case). The adiabatic step dominates the cost for realistic $\varepsilon$; the filtering step's $\kappa\ln(2/\varepsilon)$ contribution is relatively small.

The paper provides the explicit fidelity bound after filtering (from Eq. (J3) of the original paper):

$$f \geq \frac{1-\delta}{1-\delta+\delta\varepsilon_f^2}$$

which gives a tighter relation between $\varepsilon_f$ and the final error $\varepsilon$ than just $\varepsilon \leq \varepsilon_f$.

---

## Key results

**Practical constant factor (main finding):** $\alpha \approx 1.56$ for non-Hermitian $8\times 8$ matrices at $\kappa = 50$. For PD Hermitian matrices, $\alpha$ is even smaller (~0.2 for $4\times4$ PD at $\kappa = 50$). Both are about three orders of magnitude below the analytical bounds.

**Discrete vs. randomized (head-to-head):** The discrete adiabatic walk is ~20× cheaper than the randomized method in walk steps vs. evolution time. The actual advantage is larger because the randomized method's evolution time must still be converted to quantum circuit operations via [[Hamiltonian simulation]].

**Cost partition:** For $\alpha \approx 1$, the adiabatic step accounts for the majority of the total cost across all practically relevant $\varepsilon$ values. The filtering step dominates only at astronomically small $\varepsilon$ (consistent with the analysis in the original paper, which identified $\varepsilon \lesssim 10^{-180}$ as the crossover point).

---

## Comparison with prior work

| Method | Asymptotic scaling | Practical constant ($\alpha$) | Notes |
|---|---|---|---|
| [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes\|Discrete adiabatic walk (analytical)]] | $O(\kappa\log(1/\varepsilon))$ — optimal | $\alpha \sim 2305$ (PD), $\sim 5632$ (general) | Way too loose |
| Discrete adiabatic walk (numerical, **this paper**) | $O(\kappa\log(1/\varepsilon))$ — optimal | $\alpha \approx 1.56$ (general), $\sim 0.2$ (PD) | **Tight** |
| Randomized adiabatic (Jennings et al. 2023) | $O(\kappa\log(\kappa/\varepsilon))$ — suboptimal | Total time $\sim 20\times$ walk steps | Worse scaling *and* worse constant |

The conclusion is clean: the discrete method wins on both asymptotic scaling and practical constant factors. The randomized method's tighter *analytical* bound was misleading — it was tighter only because the discrete method's bound was spectacularly loose, not because the randomized method was actually better.

---

## Limits / caveats

1. **Small matrices only.** The numerical tests use $4\times4$, $8\times8$, $16\times16$ matrices. Extrapolation to cryptographically relevant sizes ($N = 2^{10}$ or larger) is assumed but not demonstrated. The authors argue the linear-$\kappa$ scaling is theoretically guaranteed, so the constant should transfer — but there could be structure-dependent effects at larger sizes.

2. **Random matrices may not represent structured problems.** The test instances are randomly generated. Real-world QLSPs (from discretized PDEs, quantum chemistry, etc.) may have spectral structure that either helps or hurts the constant. No structured benchmarks are included.

3. **$p = 1.4$ is not analytically justified.** The schedule parameter $p = 1.4$ is chosen empirically. The original paper analyzed $p = 3/2$ because it yields cleaner expressions. The optimal $p$ likely depends on $\kappa$ and the specific matrix.

4. **The comparison with the randomized method is somewhat unfair in its favour.** The randomized method's complexity is given as total evolution time, not circuit depth. Converting to actual gates via [[Hamiltonian simulation]] (e.g., [[Product Formulas]]s or LCU) would add at least a constant multiplicative factor, widening the gap.

5. **No T-count or surface code compilation.** This is a complexity comparison at the query level, not a full resource estimate. Translating to T-gates and physical qubits (as done for chemistry problems in other Babbush group papers) would be needed for practical assessment.

---

## Reusable ideas

1. [[Numerical Validation of Analytical Constant Factors]] — The practice of numerically testing the actual constant in an analytically proven bound, using circuit-level simulation on random instances. Sounds obvious, but it wasn't done in the original paper and the gap turned out to be three orders of magnitude.

2. [[Adiabatic–Filter Cost Partitioning]] — The decomposition of total QLSP cost into adiabatic and filtering components, with explicit optimization over the intermediate error parameter $\Delta$. Useful for any two-stage prepare-then-refine algorithm.

---

## References within this paper

- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa, An, Sanders, Su, Babbush, Berry (2022)]] — The original optimal QLSP solver this paper validates.
- Harrow, Hassidim, Lloyd (2009) — Original HHL algorithm.
- An, Lin (2019) — Time-optimal adiabatic QLSP.
- Ambainis (2010) — Variable-time amplitude amplification for QLSP.
- Lin, Tong (2020) — Zeno eigenstate filtering.
- Childs, Kothari, Somma (2017) — LCU-based QLSP solver.
- Harrow, Kothari (2021) — The $\Omega(\kappa\log(1/\varepsilon))$ lower bound.
- Subaşı, Somma, Orsucci (2019) — Original randomized adiabatic QLSP.
- Jennings, Lostaglio, Pallister, Sornborger, Subaşı (2023) — Improved randomized adiabatic QLSP; arXiv:2305.11352. Claimed to be faster than discrete walk for certain $\kappa$ ranges. This paper refutes that claim.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders et al. (2020)]] — Optimal probability distribution for the randomized method.
- Jansen, Ruskai, Seiler (2007) — Standard adiabatic condition.
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] — Asymmetric qubitization referenced in block-encoding construction.

---

## Cross-links

### Paper notes
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — Direct predecessor; this paper validates the practical efficiency of that algorithm.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Provides the optimal probability distribution used in testing the randomized method; also the source of the [[Stroboscopic Adiabatic Walk]].
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Cites the original QLSP solver; the constant-factor results here affect the practical cost of QLSP-based ODE approaches.
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the original HHL that started this line; the discrete adiabatic walk achieves the optimal scaling HHL's $O(\kappa^2/\varepsilon)$ left open
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — near-optimal predecessor using continuous adiabatic scheduling; this paper's discrete walk supersedes it practically
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — related Costa/Berry follow-on in the differential-equations direction; both rely on the improved QLSP lineage rather than the original HHL scaling
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — nonlinear DE solver from overlapping authors; uses the optimal QLSP solver whose practical constant is assessed here
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — later extension of the same discrete-adiabatic framework to large time steps

### Trick cards
- [[Discrete Adiabatic Theorem for Quantum Walks]] — The core theoretical tool from the predecessor paper; validated numerically here.
- [[Dolph-Chebyshev Eigenstate Filtering]] — The filtering step that boosts precision from constant to $\varepsilon$.
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — The walk operator framework.
- [[Stroboscopic Adiabatic Walk]] — The earlier heuristic version; this paper's results suggest the rigorous discrete walk has even better practical performance than expected.
- [[Numerical Validation of Analytical Constant Factors]] — New trick from this paper.
- [[Adiabatic–Filter Cost Partitioning]] — New trick from this paper.
- [[Asymmetric Block Encoding via Hadamard Pairing]] — Used in the block-encoding construction.
