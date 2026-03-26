> **Source:** Danial Motlagh and Nathan Wiebe, *Generalized Quantum Signal Processing*, arXiv:2308.01501, PRX Quantum (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2308.01501) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.5.020368)
> **Tags:** #QSP #GQSP #polynomials #block-encoding #unitary-transformation #convolution #fractional-query

---

## The computational problem

Given oracle access to a unitary $U = e^{iH}$, implement a polynomial transformation $P(U)$ as a block-encoding using controlled-$U$ operations and single-qubit rotations on an ancilla.

Standard [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] restricts $P$ to be a **real** polynomial with **definite parity** matching the degree $d \bmod 2$. To build an arbitrary complex polynomial with indefinite parity, you'd need four separate QSP instances stitched together with [[Linear Combination of Unitaries (LCU)|LCU]] and [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]], tripling the query cost.

The question: can we remove these restrictions entirely?

---

## What the paper does

Yes. GQSP replaces the single-axis rotations (X- or Z-rotations) used as signal processing operators in standard QSP with **general SU(2) rotations** on the ancilla qubit. This single change lifts both the realness and parity constraints. The only requirement left is $|P(e^{i\theta})|^2 \leq 1$ for all $\theta$ — which is forced by unitarity anyway. You can't do better than that.

The paper also provides:
1. A recursive algorithm for computing GQSP rotation angles when both $P$ and $Q$ are known — much simpler than existing QSP phase-finding
2. An FFT-based optimisation algorithm for finding $Q$ given only $P$, scaling to degree $\sim 10^7$ polynomials in under a minute on GPU
3. Applications to Hamiltonian simulation (simplified formulation), an optimal fractional query algorithm, bosonic operator implementation, and normal matrix synthesis (diagonal and circulant/convolution matrices)

**My assessment:** This is a genuinely important extension of the QSP framework. The key insight — that SU(2) signal processing operators remove all practical polynomial restrictions — is clean and natural in retrospect, but nobody had done it properly before. The paper's impact is already visible: [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes|Berry-Motlagh-Pantaleoni-Wiebe (2024)]] used GQSP immediately to halve the query count for Hamiltonian simulation. The phase-finding simplification is a real pedagogical win too — standard QSP phase-finding is notoriously fiddly, and the GQSP recursive formula is straightforward.

---

## The algorithm / construction

### Signal operator

GQSP uses a **0-controlled** application of $U$:

$$A = |0\rangle\langle 0| \otimes U + |1\rangle\langle 1| \otimes I = \begin{pmatrix} U & 0 \\ 0 & I \end{pmatrix}$$

This is simpler than standard QSP's $\sigma_z$-basis signal operator $\begin{pmatrix} e^{iH} & 0 \\ 0 & e^{-iH} \end{pmatrix}$, which requires both $U$ and $U^\dagger$. GQSP uses only forward applications of $U$.

### Signal processing operators

Instead of $R_x(\phi)$ or $R_z(\phi)$ rotations, use general SU(2) rotations:

$$R(\theta, \phi, \lambda) = \begin{pmatrix} e^{i(\lambda+\phi)}\cos\theta & e^{i\phi}\sin\theta \\ e^{i\lambda}\sin\theta & -\cos\theta \end{pmatrix} \otimes I$$

Three real parameters per rotation, vs one in standard QSP. Only the first rotation carries $\lambda$; subsequent rotations absorb it into $\phi$.

### The main theorem (Theorem 3)

Interleaving $d$ signal operators with $d+1$ SU(2) rotations:

$$\left(\prod_{j=1}^{d} R(\theta_j, \phi_j, 0)\, A\right) R(\theta_0, \phi_0, \lambda) = \begin{pmatrix} P(U) & \cdot \\ Q(U) & \cdot \end{pmatrix}$$

if and only if:
1. $P, Q \in \mathbb{C}[x]$ with $\deg(P), \deg(Q) \leq d$
2. $\forall x \in \mathbb{T}$: $|P(x)|^2 + |Q(x)|^2 = 1$

That's it. No parity constraint. No realness constraint. Complex polynomials of any degree structure.

The proof is by induction in both directions. Forward: each step multiplies by $R \cdot A$, which maps degree-$(d-1)$ polynomials $(\hat{P}, \hat{Q})$ to degree-$d$ polynomials $(P, Q)$ via:

$$P(U) = e^{i\phi}(\cos\theta \cdot U\hat{P}(U) + \sin\theta \cdot \hat{Q}(U))$$
$$Q(U) = \sin\theta \cdot U\hat{P}(U) - \cos\theta \cdot \hat{Q}(U)$$

The norm condition $|P|^2 + |Q|^2 = 1$ is preserved automatically by unitarity.

Reverse: given $(P, Q)$ of degree $d$ satisfying the norm condition, choose $\theta_d = \tan^{-1}(|b_d|/|a_d|)$ and $\phi_d = \mathrm{Arg}(a_d/b_d)$ where $a_d, b_d$ are the leading coefficients. This cancels the degree-$d$ term in one polynomial and the degree-$0$ term in the other (via a Fourier orthogonality argument on the norm condition). Recurse.

### Achievability (Theorem 4 + Corollary 5)

For any $P \in \mathbb{C}[x]$ with $|P(x)|^2 \leq 1$ on the unit circle, there exists a $Q$ with $\deg(Q) = \deg(P)$ satisfying $|P|^2 + |Q|^2 = 1$ on $\mathbb{T}$.

The proof constructs $Q$ explicitly via Riesz-Fejér-style factorisation: write $1 - |P(e^{i\theta})|^2$ as a non-negative trigonometric polynomial, factor the associated self-inversive polynomial by grouping roots into unit-circle roots (which have even multiplicity) and off-circle pairs $(w_i, 1/w_i^*)$, and extract the square root.

This means: **any polynomial bounded by 1 on the unit circle can be implemented by GQSP**, period. Compare with standard QSP in the $\sigma_z$ basis (Theorem 2), which requires $P$ to be real with parity $d \bmod 2$.

### Negative powers (Theorem 6)

To implement Laurent polynomials (negative powers of $U$), replace some signal operators $A$ with the complementary operator:

$$A' = |0\rangle\langle 0| \otimes I + |1\rangle\langle 1| \otimes U^\dagger = \begin{pmatrix} I & 0 \\ 0 & U^\dagger \end{pmatrix}$$

Since $A' = (I \otimes U^\dagger) A$ and $(I \otimes U^\dagger)$ commutes with both $A$ and $R$, replacing $k$ instances of $A$ with $A'$ multiplies both $P$ and $Q$ by $U^{-k}$, shifting the polynomial by $-k$ in degree. Same rotation angles, shifted spectrum.

---

## Key results

**Theorem 3 (GQSP).** $P, Q \in \mathbb{C}[x]$ with $\deg \leq d$ and $|P|^2 + |Q|^2 = 1$ on $\mathbb{T}$ $\iff$ constructible by $d$ controlled-$U$ operations interspersed with SU(2) rotations.

**Corollary 5.** Any $P \in \mathbb{C}[x]$ with $|P|^2 \leq 1$ on $\mathbb{T}$ can be implemented via GQSP.

**Theorem 7 (Hamiltonian simulation).** Given $U = e^{iH}$, implement $e^{it\sin H}$ and $e^{it\cos H}$ to accuracy $\epsilon$ using $O(t + \log(1/\epsilon)/\log\log(1/\epsilon))$ controlled-$U$ operations and a single ancilla qubit.

**Corollary 8 (Qubitization simulation).** Block-encode $e^{-iHt}$ using $O(\alpha t + \log(1/\epsilon)/\log\log(1/\epsilon))$ applications of the walk operator $W$, where $\alpha$ is the [[Linear Combination of Unitaries (LCU)|LCU]] 1-norm.

**Theorem 10 (Fractional queries).** Implement $U^t = e^{itH}$ for $t \in (0,1)$ to accuracy $\epsilon$ using $O(1/\delta + \log(1/\epsilon))$ controlled-$U$ operations, where $\delta = \sigma_{\min}(H)$. The $O(1/\delta)$ term matches the proved lower bound from Sheridan et al. This improves on the previous best $O((1/\delta)\log(1/\epsilon))$ — the error and gap-inversion costs are now *additive*, not multiplicative.

**Theorem 11 (Square-root phase function).** Implement $e^{it\sqrt{H}}$ using $O((1/\delta)\log(1/\epsilon) + |t|)$ controlled-$U$ operations. Relevant for bosonic operators $e^{i(a^\dagger + a)t}$.

**Lemma 12 + Theorem 13 (Normal matrix factories).** Any $N \times N$ diagonal matrix expressible as a degree-$d$ polynomial of the root-of-unity unitary $U_\omega$ can be implemented using $O(d\log N)$ 1- and 2-qubit gates.

**Theorem 17 (Convolution).** Convolve a quantum state by a filter of length $d$ using $O(d\log N + \log^2 N)$ 1- and 2-qubit gates, via circulant matrix synthesis through [[Quantum Fourier Transform Circuit|QFT]]-diagonalised cyclic permutation.

---

## Comparison with prior work

| Aspect | Standard QSP ($\sigma_x$ basis, Thm 1) | Standard QSP ($\sigma_z$ basis, Thm 2) | GQSP (Thm 3) |
|---|---|---|---|
| Polynomial type | Real $P$, real $Q$ | Real $P$ | Complex $P$, complex $Q$ |
| Parity constraint | $P$: parity $d \bmod 2$; $Q$: parity $(d-1) \bmod 2$ | $P$: parity $d \bmod 2$ | None |
| Norm condition | $\|P(x)\|^2 + (1-x^2)\|Q(x)\|^2 = 1$ on $[-1,1]$ | $\|P(x)\|^2 \leq 1$ on $\mathbb{T}$ | $\|P(x)\|^2 + \|Q(x)\|^2 = 1$ on $\mathbb{T}$ |
| Arbitrary complex poly? | Requires 4× QSP + LCU + OAA | Requires 4× QSP + LCU + OAA | Direct, single pass |
| Signal operator | $U$ and $U^\dagger$ | $U$ and $U^\dagger$ | $U$ only (or $U^\dagger$ for negative powers) |
| Phase computation | Hard ([[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|QSPPACK]] etc.) | Hard | Simple recursive formula (Alg 1) |
| GPU-scale phase finding | ~$10^4$ degree | ~$10^4$ degree | ~$10^7$ degree |

| Application | Previous best | GQSP result |
|---|---|---|
| Fractional query $U^t$ | $O((1/\delta)\log(1/\epsilon))$ — [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes\|Gilyén et al. 2019]] | $O(1/\delta + \log(1/\epsilon))$ — optimal |
| Hamiltonian simulation | Same asymptotic, but needs LCU for mixed parity | Direct single-pass via [[Jacobi-Anger Truncation for QSP Simulation\|Jacobi-Anger expansion]] |
| Convolution ($d$-length filter, $N$-dimensional state) | $O(d\log N)$ via LCU of cyclic permutations | $O(d\log N + \log^2 N)$ via GQSP of QFT-diagonalised permutation |

---

## Limits / caveats

1. **Phase-finding for $Q$ from $P$ alone.** The FFT-based optimisation (Eq. 60) is fast but is a non-convex optimisation — no convergence guarantee to the global minimum. For the tested cases (up to degree $10^7$) it works, but pathological $P$ could cause trouble. The recursive angle-finding (Algorithm 1) is exact but requires knowing $Q$ first.

2. **Numerical stability of the recursive angle extraction.** Algorithm 1 peels off angles one by one via leading-coefficient ratios. For high-degree polynomials with small leading coefficients, this could accumulate numerical errors. The paper doesn't analyse stability, and for practical deployment at degree $10^7$, this matters.

3. **No QSVT generalisation here.** GQSP handles *unitary* transformations ($U$ is unitary, $P(U)$ is a polynomial of a unitary). Extending to *singular value* transformations of block-encoded non-square matrices — the GSVT analogue — is flagged as an open problem. This limits direct applicability to problems where the input is a Hermitian block-encoding (the standard QSVT use case) rather than a unitary.

4. **Normal matrix factory applicability.** The diagonal/circulant synthesis results (Section VI) are clean but somewhat niche. The circulant result requires knowing the filter coefficients classically and having an efficient change-of-basis circuit (QFT for circulant, general $Q$ for other normal matrices). The practical impact is limited to cases where the matrix structure aligns with this framework.

5. **Simulation complexity not improved asymptotically.** The Hamiltonian simulation application (Corollary 8) has the same $O(\alpha t + \log(1/\epsilon)/\log\log(1/\epsilon))$ scaling as [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang 2017]]. The advantage is conceptual simplicity (no LCU needed for mixed parity) rather than query count. The actual $2\times$ query reduction came later in [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes|Berry et al. 2024]], which combined GQSP with [[Directional Walk Control for Phase Doubling|directional walk control]].

---

## Reusable ideas

1. [[Generalized Quantum Signal Processing (GQSP)]] — the central construction: SU(2) signal processing operators lift all parity and realness constraints on QSP polynomials.

2. [[SU(2) Response Tuple Characterization]] — the inductive proof structure: GQSP's response $(P, Q)$ satisfies $|P|^2 + |Q|^2 = 1$ on $\mathbb{T}$, proved via coefficient-level Fourier orthogonality. This characterisation is the foundation for the recursive angle-finding.

3. [[Recursive Phase Extraction from Polynomial Coefficients]] — Algorithm 1: given $(P, Q)$ satisfying the GQSP norm condition, extract rotation angles $(\theta_d, \phi_d)$ from the leading coefficients, reduce degree by 1, and recurse. Trivially correct by construction.

4. [[FFT-Based Complementary Polynomial Optimisation]] — finding $Q$ given $P$ by minimising $\|\vec{a} \star \mathrm{rev}(\vec{a})^* + \vec{b} \star \mathrm{rev}(\vec{b})^* - \vec{\delta}\|^2$ via FFT convolution. Scales to degree $10^7$.

5. [[Normal Matrix Fourier Decomposition via Root-of-Unity Unitary]] — any normal matrix $A$ in eigenbasis $\{|\lambda_j\rangle\}$ can be written as $A = \sum_n c_n U_\omega^n$ where $U_\omega = \sum_j \omega_N^j |\lambda_j\rangle\langle\lambda_j|$. Enables GQSP-based synthesis of diagonal and circulant matrices.

---

## References within this paper

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] [Ref 2] — standard QSP simulation this paper generalises
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] [Ref 3] — qubitization framework
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] [Ref 4] — LCU, which GQSP removes the need for in many contexts
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] [Ref 11] — QSVT framework, previous fractional query result
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|Dong-Meng-Whaley-Lin (2021)]] [Ref 14] — QSPPACK phase-finding, which GQSP's approach massively outscales
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes|Berry-Childs-Kothari (2015)]] [Ref 1] — optimal-parameter simulation
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] [Ref 12] — HHL, phase estimation approach to matrix functions
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn-Rossi-Tan-Chuang (2021)]] [Ref 10] — unification of quantum algorithms as eigenvalue transformations
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2017)]] [Ref 33] — QLSP solver used for convolution inversion
- de Lima Silva, Borges, Aolita (2022) [Ref 20] — Fourier-based QSP, independently used SU(2) rotations but with Laurent polynomial analysis
- Yu, Yao, Li, Wang (2022) [Ref 21] — single-qubit QNN, also used general rotations

---

## Cross-links

### Paper notes
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the standard QSP this generalises
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — the QSVT framework; GQSP lifts its polynomial restrictions
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — qubitization walk operator used in Corollary 8
- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — first major application of GQSP; combines it with directional walk control for $2\times$ simulation speedup
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — previous state-of-art for QSP phase-finding (~$10^4$ degree); GQSP's approach reaches ~$10^7$
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — alternative approach to the parity obstruction (affine compression rather than SU(2) rotations)
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]] — QSP/QSVT as unifying language; GQSP extends this further
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]] — QSP optimisation landscape analysis; relevant contrast with GQSP's simpler optimisation
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — orthogonal improvement (randomisation at QSP level)

### Trick cards
- [[Generalized Quantum Signal Processing (GQSP)]] — the central trick card (to be updated with full source attribution)
- [[SU(2) Response Tuple Characterization]] — Fourier orthogonality argument for GQSP norm condition
- [[Recursive Phase Extraction from Polynomial Coefficients]] — Algorithm 1
- [[Jacobi-Anger Truncation for QSP Simulation]] — approximation engine for the simulation application
- [[Linear Combination of Unitaries (LCU)]] — what GQSP removes the need for
- [[Oblivious Amplitude Amplification (Robust)]] — also removed by GQSP for mixed-parity polynomials
- [[Parity Shifting via Spectral Multiplication in GQSP]] — related technique used in the Berry et al. application
- [[Directional Walk Control for Phase Doubling]] — used by Berry et al. with GQSP
- [[Quantum Fourier Transform Circuit]] — change-of-basis for the convolution application
- [[Normal Matrix Fourier Decomposition via Root-of-Unity Unitary]] — new trick card from this paper
