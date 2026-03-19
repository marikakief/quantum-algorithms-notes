> **Source:** Ethan N. Epperly, Lin Lin, and Yuji Nakatsukasa, *A Theory of Quantum Subspace Diagonalization*, arXiv:2110.07492, SIAM J. Matrix Anal. Appl. **43**(3), 1263–1290 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2110.07492) · [SIAM](https://doi.org/10.1137/21M145954X)
> **Tags:** #quantum-subspace-diagonalization #Krylov #generalized-eigenvalue #perturbation-theory #thresholding #early-fault-tolerant

---

## What the paper does

Provides the first rigorous theoretical analysis of why **quantum subspace diagonalization (QSD)** works despite the severe ill-conditioning of the generalized eigenvalue problem it produces. QSD builds a Krylov-type subspace $\{e^{it_j \hat{H}}\phi_0\}$ on a quantum computer, measures the projected Hamiltonian and overlap matrices $H, S$, and solves a noisy generalized eigenvalue problem $Hc = ES c$. Classical perturbation theory predicts this should fail catastrophically when $S$ is nearly singular — but it doesn't, provided you **threshold** (truncate small eigenvalues of $S$).

The paper explains this phenomenon using new matrix perturbation results and gives end-to-end error bounds combining Krylov approximation quality with noise resilience.

## The computational problem

Estimate the ground-state energy $E_0$ of a Hamiltonian $\hat{H}$ using:
1. An initial state $\phi_0$ with overlap $\gamma_0 = \langle\psi_0|\phi_0\rangle$ (ground-state component)
2. Hamiltonian evolution $e^{it\hat{H}}$ (available on a quantum device)
3. Noisy measurements of inner products $\phi_j^*\hat{H}\phi_k$ and $\phi_j^*\phi_k$ via Hadamard tests

## The QSD algorithm

Build the unitary Krylov subspace from time-evolved states $\phi_j = e^{it_j\hat{H}}\phi_0$ for $j = -k, \ldots, k$ (so $n = 2k+1$ basis vectors). Form the $n \times n$ matrices:

$$H_{jl} = \langle\phi_j|\hat{H}|\phi_l\rangle, \quad S_{jl} = \langle\phi_j|\phi_l\rangle$$

These are measured on the quantum computer with Monte Carlo noise at level $\eta = \sqrt{\|\Delta H\|^2 + \|\Delta S\|^2}$.

Then solve the generalized eigenvalue problem $(H + \Delta H)c = E(S + \Delta S)c$ with **Löwdin thresholding** (Algorithm 1.1):

1. Diagonalise $\tilde{S} = S + \Delta S = V D V^*$
2. Keep only eigenvectors with eigenvalues $> \varepsilon$ (the threshold)
3. Solve the reduced standard eigenvalue problem in the truncated basis

## Why thresholding works (the central insight)

Classical worst-case perturbation theory (e.g., Stewart's bounds) says that for a generalized eigenvalue problem $(H, S)$ with near-singular $S$, perturbations of order $\eta$ can cause eigenvalue shifts of order $\eta/\varepsilon$ — diverging as $\varepsilon \to 0$. There exist explicit examples where this happens.

But QSD matrices are not worst-case. The paper identifies a **structural condition** that holds for QSD-generated pairs: a weighted geometric mean inequality

$$|v_i^* H v_j| \leq \mu [\min(\lambda_i, \lambda_j)]^{1-\alpha} [\max(\lambda_i, \lambda_j)]^{\alpha}$$

where $v_i$ are eigenvectors of $S$, $\lambda_i$ are the corresponding eigenvalues, and $0 \leq \alpha \leq 1/2$ (empirically $\alpha \approx 1/4$ for physical Hamiltonians). This says that the cross-terms between large and small eigenspaces of $S$ in the matrix $H$ are controlled — exactly the condition that prevents thresholding from discarding important information.

## Main results

### Perturbation bound (Theorem 2.7, informal)

Under the geometric mean condition with parameters $\mu, \alpha$, choosing threshold $\varepsilon = \Theta(\eta^{1/(1+\alpha)})$ gives:

$$|\tan^{-1}\tilde{E}_0 - \tan^{-1}E_0| \leq O\!\left(d_0^{-1} \cdot \eta^{1/(1+\alpha)}\right)$$

where $d_0$ is the condition number of the ground eigenangle and $\eta$ is the noise level.

For $\alpha = 1/4$ (typical), this gives error scaling as $\eta^{4/5}$ — much better than the $\eta/\varepsilon$ that classical worst-case theory predicts.

The analysis combines:
- **Davis–Kahan $\sin\Theta$ theorem** to control subspace rotation under perturbation
- **Mathias–Li perturbation theory** for definite matrix pairs, giving eigenangle bounds that depend on $d_0$ rather than $\varepsilon$

### Krylov approximation bound (Theorem 3.1)

For the noise-free Krylov subspace with $n = 2k+1$ time steps at spacing $\Delta t = \pi/\Delta E_M$:

$$0 \leq \tilde{E}_0 - E_0 \leq \frac{2}{|\gamma_0|^2}\left[\Delta E_{N-1}\varepsilon_{\text{total}} + 4\sum_{i=1}^{M}\Delta E_i|\gamma_i|^2\left(1 + \frac{\pi\Delta E_1}{\Delta E_M}\right)^{-2k} + \sum_{i=M+1}^{N-1}\Delta E_i|\gamma_i|^2\right]$$

where $\varepsilon_{\text{total}}$ is the sum of discarded eigenvalues of $S$ and $\Delta E_i = E_i - E_0$.

**Simplified form** (choosing $M = N-1$): the error is bounded by

$$\frac{8\Delta E_{N-1}(1 - |\gamma_0|^2)}{|\gamma_0|^2}\left(1 + \frac{\pi\Delta E_1}{\Delta E_{N-1}}\right)^{-2k}$$

which decays **exponentially** in $k$ with rate set by the ratio $\Delta E_1 / \Delta E_{N-1}$ (spectral gap relative to spectral width).

The key technical tool is a **trigonometric minimax polynomial** (Lemma 3.3) — the periodic analogue of Chebyshev polynomials — that concentrates weight at the ground eigenvalue while suppressing all others. The bound $\beta(a, k) \leq 2(1+a)^{-k}$ with $a = \pi\Delta E_1/\Delta E_M$ gives the exponential decay.

### End-to-end bound (Informal Theorem 1.3)

Combining Krylov approximation and noise perturbation:

$$|\tan^{-1}\tilde{E}_0 - \tan^{-1}E_0| \leq O\!\left(\frac{1-|\gamma_0|^2}{|\gamma_0|^2}e^{-2k\cdot O(\Delta E_1/\Delta E_{N-1})} + \left[\frac{\Delta E_{N-1}}{|\gamma_0|^2} + d_0^{-1}\right]\eta^{1/(1+\alpha)}\right)$$

First term: Krylov approximation error (exponentially decaying in $k$). Second term: noise-induced error (algebraically decaying in $\eta$ with exponent $1/(1+\alpha)$).

### General thresholding bound (Theorem 4.2)

For any Hermitian pair $(H, S)$ with $S$ PSD, if the ground eigenvector $c_0$ satisfies $2\sqrt{\varepsilon}\|c_0\| < 1$:

$$0 \leq \tilde{E}_0 - E_0 \leq \frac{\Delta E \cdot \varepsilon\|c_0\|^2}{1 - 2\sqrt{\varepsilon}\|c_0\|}$$

This is a clean statement: thresholding error is controlled by the eigenvector's norm in the $S$-basis and the threshold level.

## Practical guidance

- **Threshold selection:** $\varepsilon = \Theta(\eta^{1/(1+\alpha)})$ with $\alpha \approx 1/4$ typical, giving $\varepsilon \approx \eta^{4/5}$
- **Number of timesteps:** $k$ needs to be large enough that $e^{-2k \cdot \pi\Delta E_1/\Delta E_{N-1}}$ is small, i.e., $k = O(\Delta E_{N-1}/\Delta E_1 \cdot \log(1/\text{target error}))$
- **Initial overlap:** Need $|\gamma_0|^2$ reasonably large (say $\geq 0.1$); the bounds degrade as $|\gamma_0|^{-2}$
- **Toeplitz structure (§4.1):** With exact simulation, $H$ and $S$ are Hermitian–Toeplitz, so measuring only the first row suffices. This reduces the number of measurements from $O(n^2)$ to $O(n)$, with spectral-norm error $O(B\sqrt{n\log n/m})$ for $m$ samples per entry.

## Comparison with related methods

| Method | Ancillae | Handles noise? | Theory? | Notes |
|---|---|---|---|---|
| QPE | $O(\log(1/\varepsilon))$ | No (needs fault tolerance) | Yes | Standard but expensive |
| VQE ([[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes\|Peruzzo et al. 2014]]) | 0 | Heuristic | Limited | Barren plateaus, local minima |
| QSD (this paper) | 1 (Hadamard test) | Yes (thresholding) | **This paper** | Simple circuits, classical post-processing |
| [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes\|Lin-Tong (2022)]] | 1 | Yes (ACDF) | Yes | Different classical post-processing (CDF inversion) |
| [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes\|Lin-Tong (2020)]] | Many | Not designed for noise | Yes | Block-encoding model, prepares state |

QSD and Lin-Tong (2022) share the same quantum circuit (Hadamard test with controlled $e^{it\hat{H}}$) but differ in classical post-processing: QSD solves a generalized eigenvalue problem, Lin-Tong (2022) builds an approximate CDF. Both are designed for the early fault-tolerant regime.

## Limits / caveats

- **The $\alpha$ parameter is empirical.** The geometric mean condition holds with $\alpha = 1/2$ trivially (giving $\eta^{2/3}$ scaling), and the paper observes $\alpha \approx 1/4$ numerically. But there's no proof that $\alpha$ takes a specific value for all physical Hamiltonians.
- **The $n^3$ factor in Theorem 2.7** (from the projector perturbation bound) is likely an artefact of the proof technique. Numerics suggest the true dependence is milder.
- **Initial overlap** $|\gamma_0|^2$ must be non-negligible. For strongly correlated systems where Hartree-Fock or other simple initial states have exponentially small overlap, QSD suffers the same problem as all other methods.
- **The Krylov bound requires a spectral gap** $\Delta E_1 > 0$. For gapless systems, the exponential convergence in $k$ degrades.
- **Hamiltonian simulation overhead** is not analysed in detail; the paper treats controlled $e^{it\hat{H}}$ as a primitive.

## Reusable ideas

1. **[[Trigonometric Minimax Polynomial for Krylov Concentration]]** — the periodic Chebyshev analogue that concentrates Krylov weight at the ground eigenvalue; gives exponential decay $\beta(a,k) \leq 2(1+a)^{-k}$ with $a = \pi\Delta E_1/\Delta E_M$. The tool connecting unitary Krylov subspaces (circular spectrum) to the standard Chebyshev-based Krylov theory (real-line spectrum).

2. **[[Löwdin Thresholding for Noisy Generalized Eigenvalue Problems]]** — truncating small eigenvalues of the overlap matrix $S$ before solving the generalized eigenvalue problem. Works because QSD matrices satisfy a structural geometric-mean condition that prevents thresholding from discarding important information.

## References within this paper

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — QPE as the standard approach to eigenvalue estimation
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]] — VQE, the variational alternative
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin and Tong (2020)]] — near-optimal ground state preparation in the block-encoding model
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin and Tong (2022)]] — same quantum circuit, different classical post-processing (CDF approach)
- Parrish, McMahon (2019) — quantum filter diagonalisation, early QSD-type method
- Stair, Huang, Evangelista (2020) — multireference QSD for chemistry
- Klymko, Mejuto-Zaera, Cotton, et al. (2022) — real-time evolution approaches to ground-state energy
- Seki, Yunoki (2021) — quantum power method, related Krylov approach
- Mathias, Li (2004) — definite matrix pair perturbation theory (key classical ingredient)
- Davis, Kahan (1970) — $\sin\Theta$ theorem for subspace perturbation

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]

### Trick cards
- [[Trigonometric Minimax Polynomial for Krylov Concentration]]
- [[Löwdin Thresholding for Noisy Generalized Eigenvalue Problems]]
- [[Approximate CDF from Hadamard Tests]] — alternative post-processing for same quantum data
- [[Chebyshev Polynomial Spectral Projector]] — the real-line analogue of the trigonometric minimax polynomial
