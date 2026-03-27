> **Source:** Zhiyan Ding, Haoya Li, Lin Lin, Hongkang Ni, Lexing Ying, and Ruizhe Zhang, *Quantum Multiple Eigenvalue Gaussian filtered Search: an efficient and versatile quantum phase estimation method*, arXiv:2402.01013, Quantum **8**, 1487 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2402.01013) · [Quantum](https://quantum-journal.org/papers/q-2024-10-02-1487/)
> **Tags:** #phase-estimation #multiple-eigenvalues #early-fault-tolerant #Heisenberg-limit #Hadamard-test #Gaussian-filter #spectral-estimation

---

## The computational problem

**Multiple Eigenvalue Estimation (MEE):** Given a Hamiltonian $H \in \mathbb{C}^{M \times M}$ with $\|H\| \leq \pi$, oracle access to [[Hamiltonian simulation]] $e^{-iHt}$, and an initial state $|\psi\rangle$ with overlaps $p_m = |\langle \psi_m | \psi \rangle|^2$ on eigenstates, estimate all *dominant* eigenvalues $\{\lambda_m\}_{m \in D}$ to precision $\epsilon$.

Key parameters: maximal circuit depth $T_{\max}$ (longest single evolution time) and total evolution time $T_{\text{tot}} = \sum_n |t_n|$.

The *sufficiently dominant condition*: there exists an index set $D$ of dominant eigenvalues such that $p_{\min} := \min_{i \in D} p_i > p_{\text{tail}} := \sum_{i \notin D} p_i$. This separates signal from noise without requiring spectral gap assumptions.

## What the paper does

Introduces QMEGS — a Hadamard-test-based algorithm that estimates multiple eigenvalues simultaneously. The central idea: sample evolution times from a truncated Gaussian distribution, compute a filtered density function via simple classical post-processing, then find eigenvalues by peak-search with blocking.

QMEGS is the first algorithm to achieve both:
1. Heisenberg-limited scaling $T_{\text{tot}} = \tilde{O}(1/\epsilon)$ *without* spectral gap assumptions
2. Dramatically reduced circuit depth $T_{\max}$ down to $O(\log(1/\epsilon))$ when a spectral gap exists

This unifies the best-known results for gapped and gapless systems in a single framework, which is genuinely nice — prior methods (QCELS, MM-QCELS, ESPRIT-based approaches) each achieved one or the other but not both.

## The algorithm

### Step 1: Data generation (Hadamard test)

Sample times $\{t_n\}_{n=1}^N$ from a truncated Gaussian density:

$$a(t) = \left(1 - \int_{-\sigma T}^{\sigma T} \frac{1}{\sqrt{2\pi}\,T} e^{-s^2/(2T^2)} \, ds\right) \delta_0(t) + \frac{1}{\sqrt{2\pi}\,T} e^{-t^2/(2T^2)} \mathbf{1}_{[-\sigma T, \sigma T]}(t)$$

For each $t_n$, run the [[Approximate CDF from Hadamard Tests|Hadamard test]] circuit twice:
- With $W = I$: measure ancilla to get $X_n \in \{+1, -1\}$ estimating $\operatorname{Re}\langle\psi|e^{-iHt_n}|\psi\rangle$
- With $W = S^\dagger$: get $Y_n$ estimating the imaginary part
- Set $Z_n = X_n + i Y_n$, so $\mathbb{E}[Z_n] = \langle\psi|e^{-iHt_n}|\psi\rangle$

### Step 2: Compute filtered density

Evaluate the empirical filtered density on a discrete grid $\theta_j = -\pi + jq/T$ for $j = 0, \ldots, \lfloor 2\pi T/q \rfloor$:

$$G(\theta_j) = \left|\frac{1}{N} \sum_{n=1}^N Z_n \, e^{i\theta_j t_n}\right|$$

The expectation of this (before taking absolute value) is:

$$\hat{G}(\theta) = \sum_{m=1}^M p_m F(\theta - \lambda_m)$$

where $F(x) = \int a(t) e^{ixt}\,dt$ is the Fourier transform of $a(t)$. For the Gaussian choice, $F(x) \approx e^{-T^2 x^2/2}$ — a Gaussian bump concentrated near $x = 0$. So $\hat{G}(\theta)$ is a sum of Gaussian bumps centred at each eigenvalue, with heights proportional to overlaps $p_m$.

### Step 3: Peak search with blocking

Find eigenvalues by greedy peak-finding:
1. Find $\theta_1 = \arg\max_j G(\theta_j)$
2. Block an interval $(\theta_1 - \alpha/T, \theta_1 + \alpha/T)$ around it
3. Find $\theta_2 = \arg\max_{j \notin \text{blocked}} G(\theta_j)$
4. Block around $\theta_2$, repeat $K$ times

The blocking radius $\alpha/T$ prevents re-detecting the same peak. Output $\{\theta_k\}_{k=1}^K$ as estimated eigenvalues.

## Key results

**Theorem 3.1 (Gapless — Heisenberg limit):** For any $T > 0$, assuming $p_{\min} > p_{\text{tail}}$ and $K \geq |D|$, with probability $\geq 1 - \eta$: for each $i \in D$ there exists $k_i$ such that

$$|\lambda_i - \theta_{k_i}| \leq \alpha / T$$

Quantum costs to achieve $\epsilon$-covering of all dominant eigenvalues:

$$T_{\max} = \tilde{\Theta}(1/\epsilon), \qquad T_{\text{tot}} = \tilde{\Theta}\!\left(\frac{1}{(p_{\min} - p_{\text{tail}})^2 \epsilon} \cdot \log(|D|/\eta)\right)$$

This is Heisenberg-limited with no spectral gap assumption. The dominant-tail gap $p_{\min} - p_{\text{tail}}$ controls the sample complexity.

**Theorem 3.2 (Gapped among dominants — short depth):** When $T \gg \Delta_{\text{dom}}^{-1}$ (the gap among dominant eigenvalues), the algorithm establishes a *one-to-one correspondence* between detected peaks and true eigenvalues:

$$T_{\max} = \tilde{\Theta}\!\left(\frac{p_{\text{tail}}}{p_{\min}\,\epsilon}\right), \qquad T_{\text{tot}} = \tilde{\Theta}\!\left(\frac{1}{p_{\text{tail}}\,p_{\min}\,\epsilon} \cdot \log(|D|/\eta)\right)$$

For small $p_{\text{tail}}$, $T_{\max}$ scales sublinearly in $1/\epsilon$ — the gap buys you shorter circuits.

**Theorem 3.3 (Large gap to tail — constant depth):** When $T \gg \Delta^{-1}$ (gap between dominants and the rest of the spectrum) and choosing $\delta = \Omega(p_{\min}\epsilon/\Delta)$:

$$T_{\max} = \Theta\!\left(\frac{\delta}{p_{\min}\,\epsilon} \cdot \log(1/\delta)\right), \qquad T_{\text{tot}} = \tilde{\Theta}\!\left(\frac{1}{p_{\min}\,\delta\,\epsilon} \cdot \log(|D|/\eta)\right)$$

In the most favourable case, $T_{\max}$ can be reduced to $O(\log(1/\epsilon))$ — exponentially better than standard QPE.

## Comparison with prior work

| Method | $T_{\max}$ (gapless) | $T_{\max}$ (gapped) | Multi-eigenvalue | Heisenberg total |
|---|---|---|---|---|
| Textbook QPE | $O(1/\epsilon)$ | $O(1/\epsilon)$ | Yes | Yes |
| [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes\|Lin-Tong (2022)]] | $\tilde{O}(1/\epsilon)$ | — | No (ground state only) | Yes |
| [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes\|QET-U (Dong-Lin-Tong 2022)]] | $\tilde{O}(1/\epsilon)$ | — | No | Yes |
| QCELS (Ding-Lin 2023) | $\tilde{O}(1/\epsilon)$ | $O(\log(1/\epsilon))$ | No | Yes |
| MM-QCELS (Ding-Lin 2023) | — | $O(\log(1/\epsilon))$ | Yes | No (gap required) |
| ESPRIT-based (Li-Ni-Ying 2023) | $\tilde{O}(1/\epsilon)$ | — | Yes | Yes |
| **QMEGS** | $\tilde{O}(1/\epsilon)$ | $O(\log(1/\epsilon))$ | **Yes** | **Yes** |

QMEGS is the first to tick all four boxes. The Lin group has been steadily building toward this — QCELS handled the single-eigenvalue case, MM-QCELS extended to multiple eigenvalues but needed a gap, and now QMEGS completes the picture.

## Limits / caveats

- The **sufficiently dominant condition** $p_{\min} > p_{\text{tail}}$ is a real assumption. If the initial state has small overlap with a target eigenvalue relative to the total tail weight, that eigenvalue won't be detected. This is analogous to the overlap requirements in all QPE variants, but stated differently.
- The $\tilde{O}$ notation throughout hides polylog factors in $1/(p_{\min} - p_{\text{tail}})$, $|D|$, $1/\eta$, etc. For systems where $p_{\min} - p_{\text{tail}}$ is small, the constants can be substantial.
- The algorithm returns peak locations on a discrete grid with blocking — it's not producing arbitrarily precise eigenvalue estimates in a single pass. For high precision you need large $T$, and the grid search cost grows as $O(T/q)$.
- Extension to integer-time access (powers $e^{-inH}$ rather than continuous $t$) is discussed in Appendix C but adds complications — the truncated Gaussian must be replaced by a discrete distribution, and the analysis gets messier.
- Numerical experiments are on small systems ($\leq 4$ qubits). The transition from theory to practical chemistry/materials Hamiltonians remains to be validated.

## Reusable ideas

1. [[Gaussian Time-Sampling Filter for Spectral Estimation]] — sample evolution times from a (truncated) Gaussian to create Gaussian-shaped bumps in the filtered density function. The Fourier transform of a Gaussian is a Gaussian, giving spatially localised spectral peaks with controllable width $\sim 1/T$.

2. [[Greedy Peak Search with Blocking]] — after finding a spectral peak, block an interval around it before searching for the next one. Simple, effective way to resolve multiple eigenvalues from a single dataset without matrix pencil or ESPRIT-style linear algebra.

3. [[Dominant-Tail Separation Condition]] — replace traditional spectral gap assumptions with a condition on the initial state overlaps: $p_{\min} > p_{\text{tail}}$. This is a weaker and more operational assumption that applies even when eigenvalues are arbitrarily close together.

## References within this paper

- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]] — Heisenberg-limited ground state energy estimation via ACDF; precursor using the same Hadamard-test infrastructure
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong-Lin-Tong (2022)]] — QET-U for ground-state energy; complementary control-free approach
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes|Ding-Chen-Lin (2023)]] — Lindbladian ground-state prep from same group (Zhiyan Ding)
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes|Epperly-Lin-Nakatsukasa (2022)]] — quantum subspace diagonalisation theory; related Krylov approach
- Wan-Berta-Campbell (2022) — randomised statistical phase estimation; achieves Heisenberg limit for single eigenvalue
- Wang-França-Zhang-Zhu-Johnson (2023) — exponentially improved circuit depth dependence for ground-state energy via ESPRIT
- Ding-Lin (2023) — QCELS and MM-QCELS; direct precursors to QMEGS for single and multiple eigenvalues
- Li-Ni-Ying (2023) — ESPRIT-based low-depth QPE; adaptive multi-eigenvalue estimation
- Dutkiewicz-Terhal-O'Brien (2022) — Heisenberg-limited QPE of multiple eigenvalues with few control qubits
- Somma (2019) — eigenvalue estimation via time series analysis; early Hadamard-test-based spectral approach
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — original phase estimation algorithm
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — textbook QPE

## Cross-links

### Paper notes
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]

### Trick cards
- [[Approximate CDF from Hadamard Tests]]
- [[Gaussian Time-Sampling Filter for Spectral Estimation]]
- [[Greedy Peak Search with Blocking]]
- [[Dominant-Tail Separation Condition]]
