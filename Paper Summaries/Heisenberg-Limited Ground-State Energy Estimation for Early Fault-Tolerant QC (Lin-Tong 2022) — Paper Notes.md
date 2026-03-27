> **Source:** Lin Lin and Yu Tong, *Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant Quantum Computers*, arXiv:2102.11340, PRX Quantum **3**, 010318 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2102.11340) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.3.010318)
> **Tags:** #ground-state-energy #phase-estimation #early-fault-tolerant #Heisenberg-limit #spectral-measure #QET-U

---

## What the paper does

Estimates the ground-state energy of a Hamiltonian $H$ with **Heisenberg-limited precision** ($\varepsilon^{-1}$ scaling in total evolution time) using only **one ancilla qubit** and **short maximal evolution time** $\tilde{O}(\varepsilon^{-1})$. This is specifically designed for **early fault-tolerant** quantum computers — devices with enough error correction for coherent circuits of moderate depth, but not the deep circuits and many ancillas needed for textbook QPE.

The algorithm also produces an **approximate cumulative distribution function (ACDF)** of the spectral measure, which can be reused for other spectral properties.

## The problem

Given $H = \sum_{k=0}^{K-1} \lambda_k \Pi_k$ with eigenvalues $\lambda_0 < \lambda_1 \leq \cdots$ and an initial state $\rho$ with ground-state overlap $p_0 = \operatorname{Tr}[\rho \Pi_0] \geq \eta > 0$ (where $\eta$ is a known lower bound), estimate $\lambda_0$ to additive error $\varepsilon$ with success probability $\geq 1 - \vartheta$.

Three resource measures matter:
- **Maximal evolution time** $T_{\max}$: the longest single coherent Hamiltonian evolution in any circuit execution
- **Total evolution time** $T_{\text{tot}}$: the sum of all evolution times across all circuit executions
- **Number of repetitions** $M$: how many times the circuit is run

The ideal algorithm achieves: $T_{\max} = \tilde{O}(\varepsilon^{-1})$ (Heisenberg scaling), $T_{\text{tot}} = \tilde{O}(\varepsilon^{-1} \eta^{-2})$, single ancilla qubit, no inverse QFT.

## Why this is hard

Textbook QPE achieves Heisenberg-limited precision but needs:
- $O(\log(1/\varepsilon))$ ancilla qubits
- Inverse quantum Fourier transform circuit
- $T_{\max} = O(\varepsilon^{-1})$ but $T_{\text{tot}} = O(\varepsilon^{-1} p_0^{-2})$ or worse

Semi-classical QPE (Kitaev's version with one ancilla) needs $T_{\max} = O(\varepsilon^{-1} p_0^{-1})$ — the maximal evolution time blows up when the initial overlap is small. That's the bottleneck: to resolve the ground state from a noisy initial state, you need longer individual circuits.

## The construction

### Step 1: Sample from a simple Hadamard-test circuit

The quantum circuit is just a Hadamard test with one ancilla:

$$|0\rangle \xrightarrow{H} \xrightarrow{W} \xrightarrow{\text{ctrl-}e^{-ij\tau H}} \xrightarrow{H} \text{measure}$$

where $W = I$ gives $\mathbb{E}[X_j] = \operatorname{Re}\operatorname{Tr}[\rho\, e^{-ij\tau H}]$ and $W = S^\dagger$ gives $\mathbb{E}[Y_j] = \operatorname{Im}\operatorname{Tr}[\rho\, e^{-ij\tau H}]$.

Here $\tau = \Theta(\|H\|^{-1})$ is chosen so $\tau\|H\| < \pi/3$, and $j$ ranges over $\{-d, \ldots, d\}$ with $d = O(\delta^{-1}\log(\delta^{-1}\eta^{-1}))$. The maximal evolution time is $\tau d = O(\varepsilon^{-1}\text{polylog}(\varepsilon^{-1}\eta^{-1}))$.

### Step 2: Construct the approximate CDF via importance sampling

Define the spectral measure $p(x) = \sum_k p_k \delta(x - \tau\lambda_k)$ and the CDF $C(x) = (H * p)(x)$ where $H$ is the periodic Heaviside function.

Approximate $H$ by a trigonometric polynomial $F(x) = \sum_{|j| \leq d} \hat{F}_j e^{ijx}$ (a smoothed step function with degree $d$ and transition width $\delta$). The ACDF is:

$$\tilde{C}(x) = \sum_{|j| \leq d} \hat{F}_j e^{ijx} \operatorname{Tr}[\rho\, e^{-ij\tau H}]$$

which satisfies $C(x - \delta) - \varepsilon_F \leq \tilde{C}(x) \leq C(x + \delta) + \varepsilon_F$.

The key trick: instead of estimating each Fourier coefficient separately (which would cost $O(d)$ circuits), use **importance sampling** over $j$. Sample $J$ with $\Pr[J=j] = |\hat{F}_j| / F_1$ where $F_1 = \sum |\hat{F}_j|$, run the Hadamard test at time $|J|\tau$, and form the random variable:

$$G(x; J, Z) = F_1 \cdot Z \cdot e^{i(\theta_J + Jx)}, \quad Z = X_J + iY_J$$

This is an unbiased estimator of $\tilde{C}(x)$ with variance $\leq 2F_1^2 = O(\log^2 d)$. The $O(\log^2 d)$ variance is what makes the algorithm efficient — it means you need $O(\eta^{-2}\log^2 d)$ samples total to estimate $\tilde{C}(x)$ to accuracy $\eta/4$ (enough to distinguish "CDF $< \eta$" from "CDF $> \eta/2$").

### Step 3: Binary search via CERTIFY

The ground-state energy is where $C(x)$ jumps from $0$ to $\geq p_0$. Binary search over $x \in [-\pi/3, \pi/3]$ with a **CERTIFY** subroutine that, for a given $x$, decides whether $C(x+\delta) > \eta/2$ or $C(x-\delta) < \eta$, using the ACDF estimator with majority voting.

Algorithm INVERT_CDF:
1. Initialize $x_0 = -\pi/3$, $x_1 = \pi/3$
2. Repeat $O(\log(\delta^{-1}))$ times: set $x_{\text{mid}} = (x_0 + x_1)/2$, call CERTIFY($x_{\text{mid}}$)
3. If CERTIFY returns "below" ($C < \eta$), set $x_0 = x_{\text{mid}}$; if "above" ($C > \eta/2$), set $x_1 = x_{\text{mid}}$
4. Return $x_1 / \tau$ as the energy estimate

## Key results

**Theorem 2 (CDF Inversion):** With $d = O(\delta^{-1}\log(\delta^{-1}\eta^{-1}))$ and $M = O(\eta^{-2}\log^2(d)(\log\log(\delta^{-1}) + \log(\vartheta^{-1})))$ samples:
- Expected total evolution time: $\tilde{O}(\tau\delta^{-1}\eta^{-2}\log(\vartheta^{-1}))$
- Maximal evolution time: $O(\tau\delta^{-1}\log(\delta^{-1}\eta^{-1}))$
- Classical post-processing: $\tilde{O}(\eta^{-2}\log^3(\delta^{-1})\log(\vartheta^{-1}))$

**Corollary 3 (Ground-State Energy):** Setting $\delta = \varepsilon\tau$:
- Expected total evolution time: $\tilde{O}(\varepsilon^{-1}\eta^{-2})$ — **Heisenberg-limited** in $\varepsilon$
- Maximal evolution time: $O(\varepsilon^{-1}\text{polylog}(\varepsilon^{-1}\eta^{-1}))$ — polylog overhead over Heisenberg
- Number of measurements: $\tilde{O}(\eta^{-2}\text{polylog}(\varepsilon^{-1}))$
- Ancilla qubits: **1**

## Comparison with prior work

| Method | Ancillae | $T_{\max}$ | Total evolution time | Notes |
|---|---|---|---|---|
| Textbook QPE | $O(\log(1/\varepsilon))$ | $O(\varepsilon^{-1})$ | $\tilde{O}(\varepsilon^{-1}\eta^{-2})$ | Needs QFT; subtle $\eta$ subtleties |
| Semi-classical QPE (1 ancilla) | 1 | $\tilde{O}(\varepsilon^{-1}\eta^{-1})$ | $\tilde{O}(\varepsilon^{-1}\eta^{-2})$ | $T_{\max}$ blows up with small $\eta$ |
| QEEA (Somma 2019) | 1 | $\tilde{O}(\varepsilon^{-1}\text{polylog}(\eta^{-1}))$ | $\tilde{O}(\varepsilon^{-4}\eta^{-2})$ | Heisenberg $T_{\max}$ but $\varepsilon^{-4}$ total! |
| **This work** | **1** | $\tilde{O}(\varepsilon^{-1}\text{polylog}(\eta^{-1}))$ | $\tilde{O}(\varepsilon^{-1}\eta^{-2})$ | **All three properties** |
| [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes\|Lin-Tong (2020)]] | many | $O(\alpha/(\gamma\Delta))$ | $O(\alpha/(\gamma\Delta)\cdot\log(1/\varepsilon))$ | Block-encoding model; prepares state, not just energy |

This is the first algorithm to simultaneously achieve: (1) Heisenberg-limited total evolution time, (2) polylog-overhead maximal evolution time, and (3) single ancilla qubit.

## The ACDF as a standalone tool

Beyond ground-state energy, the ACDF $\tilde{C}(x)$ gives you the full spectral distribution of $H$ with respect to $\rho$. From it you can extract:
- Spectral gaps (where $C$ is flat)
- Weight in energy windows (differences of CDF values)
- Excited-state energies (other jump locations)
- Density of states estimates

This makes the algorithm a spectral Swiss army knife, not just a ground-state energy estimator.

## Connection to QET-U

The paper's approach is closely related to **quantum eigenvalue transformation of unitary matrices (QET-U)**. The Hadamard test circuit with controlled $e^{-ij\tau H}$ is the input model for QET-U. The Fourier coefficients $\hat{F}_j$ define a polynomial transformation of eigenvalues, and the importance sampling is a randomised way to implement this transformation without actually running the full QET-U circuit.

The follow-up by Dong, Lin, Ni, and Wang (2022) makes this connection explicit and develops more algorithms in the QET-U framework.

## Limits / caveats

- **Overlap requirement:** You must know a lower bound $\eta$ on $p_0 = \langle\psi_0|\rho|\psi_0\rangle$. If $\eta$ is exponentially small, the $\eta^{-2}$ in the total evolution time kills you. This is the same wall every QPE-based method hits — there's no getting around it without better state preparation.
- **Total evolution time still $\eta^{-2}$:** The Heisenberg limit is in $\varepsilon$, not in $\eta$. The $\eta^{-2}$ factor matches QPE but doesn't beat it. Amplitude amplification in the block-encoding model (Lin-Tong 2020) reduces this to $\eta^{-1}$ but needs more qubits.
- **Classical post-processing:** The binary search + majority voting has $\tilde{O}(\eta^{-2}\text{polylog})$ classical cost, which is modest.
- **[[Hamiltonian simulation]] overhead:** The circuit requires controlled $e^{-ij\tau H}$, which itself needs a [[Hamiltonian simulation]] subroutine. The paper analyses Trotter-based implementation (Appendix D), adding polynomial overhead in system size and sparsity. More advanced simulation methods (Taylor series, QSP) can reduce this.
- **Not a state preparation algorithm:** Unlike Lin-Tong (2020), this only estimates the energy — it doesn't produce the ground state. If you need the state, you need a different approach.

## Reusable ideas

1. **[[Importance Sampling for Fourier CDF Estimation]]** — evaluating a trigonometric polynomial of eigenvalues via random selection of Fourier indices, avoiding term-by-term estimation. The variance scales as $O(\log^2 d)$ thanks to the rapid decay of Fourier coefficients of the smoothed step function.

2. **[[Approximate CDF from Hadamard Tests]]** — constructing a smooth approximation to the spectral CDF from Hadamard test measurements, then inverting it via binary search. Separates the quantum sampling (simple, shallow circuits) from the classical inversion (binary search + majority voting).

## References within this paper

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation; the semi-classical variant with 1 ancilla is the closest ancestor
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin and Tong (2020)]] — the authors' earlier block-encoding approach; prepares the state, but needs many ancillae
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT framework; QET-U connection
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn et al. (2021)]] — QSP/QET-U tutorial
- Somma (2019) — QEEA: achieves small $T_{\max}$ but at $\varepsilon^{-4}$ total cost; improved in this paper's Appendix C to $\varepsilon^{-4}$
- Dong, Lin, Ni, Wang (2022) — follow-up developing QET-U framework further (not yet in vault)
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard et al. (2002)]] — amplitude estimation used in related approaches

## Cross-links

### Paper notes
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]] — same quantum circuit, different classical post-processing (generalized eigenvalue problem vs CDF inversion)

- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — follow-up replacing block encoding with Hamiltonian evolution input model; dramatically improves QEEA total cost from $\tilde{O}(\varepsilon^{-4}\gamma^{-4})$ to $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$

- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]] — extends the Hadamard-test approach to multiple eigenvalue estimation with Gaussian time-sampling; achieves Heisenberg limit without spectral gap

### Trick cards
- [[Importance Sampling for Fourier CDF Estimation]]
- [[Approximate CDF from Hadamard Tests]]
- [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]]
- [[Gaussian Time-Sampling Filter for Spectral Estimation]]
