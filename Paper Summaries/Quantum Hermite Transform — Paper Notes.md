
> **Paper:** Efficient Quantum Hermite Transform
> **arXiv:** [2510.04929](https://arxiv.org/abs/2510.04929)
> **Authors:** Siddhartha Jain, Vishnu Iyer, Rolando D. Somma, Ning Bao, Stephen Jordan (Google Quantum AI / UT Austin / Northeastern / Brookhaven)
> **Date read:** 2026-03-12
> **Relevance:** New quantum algorithmic primitive — potential tool for Gaussian-weighted computational geometry problems
> **Back to index:** [[Master - QC in Computational Geometry]]

---

## One-Line Take

The Gaussian analogue of the QFT: polylogarithmic in dimension, exponential speedup over classical, with provable query advantages for property testing and learning under Gaussian measure.

---

## What It Does

The **Quantum Hermite Transform (QHT)** performs a change of basis from computational basis states to Hermite function states:

$$\sum_{n=0}^{N-1} \alpha_n |n\rangle \;\mapsto\; \sum_{n=0}^{N-1} \alpha_n |\psi_n\rangle$$

where $|\psi_n\rangle$ is a discretisation of the $n$-th Hermite function $\psi_n(x) = \frac{(-1)^n}{\sqrt{2^n n! \sqrt{\pi}}} e^{-x^2/2} H_n(x)$.

**Complexity:** $O\!\left((\log N + \log(1/\varepsilon))^3 \cdot \log(1/\varepsilon)\right)$ gates (Theorem 2, informal; the formal bound appears as Theorem 19 in the paper).

**Classical equivalent:** $\Omega(N)$ just to read/write the output. The QHT is an exponential speedup in dimension, same flavour as QFT vs DFT. Note: like QFT, this is a complexity speedup in the circuit sense; the Hermite transform on specific input states can still require $\Omega(1/|\hat{f}(v)|^2)$ repetitions to extract individual coefficients.

---

## Key Technical Results

### 1. Exponential Fast-Forwarding of the Quantum Harmonic Oscillator (Theorem 1)

The discrete QHO evolution $e^{-i\bar{H}t}$ can be simulated in the low-energy subspace (first $N$ eigenvectors) using **$O(\log^2 N)$ gates**, with error $O(e^{-N/10})$.

This dramatically improves Somma's prior result (2016), which achieved only subexponential fast-forwarding ($\sim e^{\sqrt{\log N}}$ complexity) via Trotter-Suzuki formulas. The key is an algebraic factorization using the $\mathfrak{sl}(2, \mathbb{R})$ Lie algebra structure (see Appendix A of the paper for the exact decomposition):

$$e^{-i\hat{H}t} = e^{-i\frac{\tan(t/2)}{2}\hat{p}^2} \cdot e^{-i\frac{\sin t}{2}\hat{x}^2} \cdot e^{-i\frac{\tan(t/2)}{2}\hat{p}^2}$$

Higher-order BCH terms vanish exactly in the continuum. In the discrete case, they show the commutator tails are doubly-exponentially small on the low-energy subspace.

### 2. QHT Algorithm (Theorem 2, informal; Theorem 19, formal)

Four-step pipeline:
1. **State preparation:** Prepare Plancherel-Rotach approximate Hermite states $|\phi_n\rangle$ using coherent arithmetic. These have constant overlap with true Hermite states $|\psi_n\rangle$ (the Plancherel-Rotach asymptotics give a good approximation in the oscillatory region for large $n$). Cost: $O((\log N + \log(1/\varepsilon))^3)$.
2. **Eigenstate filtering:** Fast QPE powered by the fast-forwarding result. The QHO eigenvalues are $n + 1/2$, so QPE on $e^{-i\bar{H}t}$ with $t = O(\log N)$ resolves eigenspaces without long simulation. Filters to $|\psi_n\rangle$. Cost: $O((\log N + \log(1/\varepsilon))^3)$.
3. **Fixed-point [[Standard Amplitude Amplification|amplitude amplification]]:** Boosts overlap to $1-\varepsilon$ without needing to know the exact initial overlap. Adds $O(\log(1/\varepsilon))$ multiplicative factor.
4. **Uncomputation:** Inverse QPE extracts $n$ from $|\psi_n\rangle$ (eigenvalue $n+1/2$), resets ancilla. This is what makes the transform a proper unitary — not just a one-shot filter.

### 3. Hermite Sampling

Given oracle access to $f: \mathbb{R}^n \to \mathbb{R}$, sample from the Hermite spectrum — output multi-index $v \in \mathbb{N}^n$ with probability $\propto |\hat{f}(v)|^2$.

- **Boolean functions:** $O(n \cdot \text{polylog}(n, d, 1/\varepsilon))$ time, where $d$ = polynomial degree cutoff
- **General functions:** Additional factor $\kappa = \|f\sqrt{\nu}\|_\infty / \|f\sqrt{\nu}\|_2$ (distortion parameter; $\nu$ = Gaussian measure weight)

---

## Provable Quantum Advantages

| Problem | Quantum | Classical | Gap |
|---------|---------|-----------|-----|
| Testing $k$-product sign functions | $O(1/\varepsilon^2)$ queries | $\Omega(k)$ queries | Removes $k$-dependence |
| Gaussian Goldreich-Levin (heavy Hermite coefficients) | $O(\kappa/\tau^2)$ — no $n$-dependence | $\Omega(n)$ queries | Removes $n$-dependence |
| Tolerant low-degree testing | $O(\log(1/\delta)/\varepsilon^2)$ | $\Omega_\varepsilon(d)$ | Removes $d$-dependence |
| Agnostic learning of $s$-sparse concepts | $\text{poly}(s\kappa/\varepsilon)$ | $\text{poly}(ns\gamma/\varepsilon)$ | Removes $n$-dependence |

**Pattern:** The QHT removes dependence on the ambient dimension $n$ or the structural parameter ($k$, $d$) in query complexity. This is the same pattern as QFT-based algorithms removing dependence on the period length.

---

## Why This Matters for Computational Geometry

The QHT is a new algorithmic primitive, not just a speedup on an existing subroutine. Analogous to how the QFT enabled entirely new algorithm families (period-finding → Shor, hidden subgroup, etc.), the QHT enables algorithms for problems where the natural basis is **Hermite functions** — i.e., problems involving:

- Gaussian measures and Gaussian-weighted integrals
- Polynomial functions weighted by $e^{-\|x\|^2}$
- Spectral analysis of operators with harmonic oscillator structure
- Property testing / learning under Gaussian input distribution

**Potential computational geometry connections:**
- [[Gaussian Intrinsic Volumes of Polytopes]] — Hermite expansion of support functions
- [[Gaussian Mixed Volumes]] — Gaussian-weighted mixed volume theory
- Geometric property testing (convexity, proximity to simplices) under Gaussian measure
- Gaussian surface area computation

---

## Caveats

- The QHT speedup is in the **transform step**, not necessarily the end-to-end algorithm. Need to check that the transform is the bottleneck for any given application.
- The distortion parameter $\kappa$ can be large for general functions, eating into the speedup.
- Hermite sampling gives spectral *samples*, not individual coefficients. Extracting specific coefficients requires $O(1/|\hat{f}(v)|^2)$ repetitions — potentially exponential if the coefficient is small.
- All demonstrated advantages are in the **query model** under Gaussian measure. Need to verify that computational geometry problems have efficient oracle implementations.

---

## Open Questions (For Our Purposes)

1. Can geometric predicates (membership, circumsphere test) be efficiently implemented as oracles in the QHT framework?
2. Does the support function of a polytope with $n$ vertices have a structured/sparse Hermite expansion?
3. Is there a formal connection between Gaussian intrinsic volumes and the Hermite spectrum of $h_P$?
4. Can the QHT help with the sign problem in mixed volumes by working in Gaussian measure instead of Lebesgue?

## References within this paper

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]] framework
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — the original hidden subgroup algorithm that started the Fourier-sampling tradition
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — the QFT-based approach to period finding that started the Fourier-sampling tradition

---

## Cross-References

- [[Hamiltonian Simulation — Comparison Tables]]
