# qDRIFT Randomized Hamiltonian Simulation (Campbell 2018)

> **Source:** Earl T. Campbell, *A random compiler for fast [[Hamiltonian simulation]]*, arXiv:1811.08017, Phys. Rev. Lett. **123**, 070503 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1811.08017) · [PRL](https://doi.org/10.1103/PhysRevLett.123.070503)
> **Tags:** #hamiltonian-simulation #qdrift #randomization #product-formulas

---

## What the paper does

Replaces deterministic Trotter ordering with a randomized compiler: at each step, sample one Hamiltonian term with probability proportional to its coefficient magnitude and apply a fixed-angle evolution under that term. Cost is then controlled by
$$
\lambda = \sum_j |h_j|
$$
rather than explicitly by the term count $L$ or the largest coefficient $\Lambda$. For dense chemistry Hamiltonian instances where $\lambda \ll \Lambda L$, this can be a meaningful saving.

## The protocol

For $H = \sum_j h_j H_j$ with $\|H_j\| = 1$, define
$$
p_j = \frac{|h_j|}{\lambda}, \qquad \tau = \frac{t\lambda}{N}.
$$
Repeat $N$ times: sample $j \sim p_j$, apply $e^{-i\tau\,\mathrm{sgn}(h_j) H_j}$.

The guarantee is for the averaged channel over the random choices, not for every individual sampled circuit. Diamond-norm error $\epsilon$ is achieved with
$$
N = O\!\left(\frac{(\lambda t)^2}{\epsilon}\right).
$$

The paper's bound can be read as \(N\) proportional to \(2(\lambda t)^2/\epsilon\) under the usual normalization; the important point is that this \(N\) is independent of \(L\) and \(\Lambda\) separately, except through their contribution to \(\lambda\).

Proof idea: one sampled micro-step has the same first-order generator as \(e^{-iH t/N}\) after averaging over \(j\). The per-step channel error is \(O((\lambda t/N)^2)\), and telescoping/subadditivity of the diamond norm over \(N\) independent averaged steps gives \(O((\lambda t)^2/N)\).

## How it relates to prior randomized compiling

The idea of using randomized circuits to reduce effective Hamiltonian cost was explored earlier (see Poulin, Qarry, Somma, Verstraete 2011 for a related random-walk approach), but qDRIFT gives a clean formulation where sampling probabilities are exactly proportional to term weights and the step size is uniform. The $\lambda$-scaling is explicit and the analysis is via diamond-norm channel composition.

## Comparison with deterministic [[Product Formulas]]s

| Method | Cost driver | Strength |
|---|---|---|
| Trotter / Suzuki | Commutators, $L$, local structure | Good when locality/ordering help |
| qDRIFT | $\lambda$ | Good when dense sums make deterministic formulas awkward |
| Qubitization / QSP | Near-optimal asymptotics | Best asymptotically, heavier machinery |

The gain over deterministic Trotter is real when $\lambda$ is meaningfully smaller than $\Lambda L$ and when deterministic formulas cannot exploit strong commutator/locality structure. This can occur in second-quantized chemistry Hamiltonians but is not guaranteed. For adversarial, sparse, or highly local Hamiltonians, the advantage can disappear.

## Limits

- If $\lambda \sim \Lambda L$, the advantage largely goes away.
- The guarantee is about the expected channel, not about individual sample paths.
- The $(\lambda t)^2/\epsilon$ scaling loses to qubitization/QSP in the high-precision or large-$t$ regime.
- Randomization introduces shot noise: not every run gives the same circuit.

## Reusable trick

The abstraction worth keeping: when a Hamiltonian is a weighted sum of many easy terms, sample terms proportional to their weight and use many fixed-angle random steps rather than one carefully designed deterministic ordering. Cost then tracks the $\ell_1$ norm, not the maximum coefficient times the term count.

## References within this paper

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — deterministic [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki product formulas]] that qDRIFT offers an alternative to
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization/QSP as the asymptotic competitor
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin, Qarry, Somma & Verstraete (2011)]] — [[Monte Carlo Average-Hamiltonian Product Formula|randomised product formula]] for arbitrary time-dependent Hamiltonians; precursor to qDRIFT's randomisation philosophy
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — Trotter error theory (comparison point)

---

## Cross-links

- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[Fixed-Angle Randomized Product Formula]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Error-Budget Allocation Across Controlled-Time Schedules]]
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — qFLO: applies Richardson extrapolation on top of qDRIFT to get depth $O((\lambda T)^2 \log(1/\varepsilon))$
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — higher-order randomized simulation via multi-[[Product Formulas]] importance sampling; direct generalization of qDRIFT's randomization idea
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — generalizes qDRIFT to time-dependent Hamiltonians via continuous sampling; universality theorem shows same complexity; also proves factor-of-2 improvement in the original qDRIFT constant
