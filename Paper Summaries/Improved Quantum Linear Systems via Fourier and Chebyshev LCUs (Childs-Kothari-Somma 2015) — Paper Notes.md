> **Source:** Andrew M. Childs, Robin Kothari, and Rolando D. Somma, *Quantum algorithm for systems of linear equations with exponentially improved dependence on precision*, arXiv:1511.02306, SIAM J. Comput. **46**, 1920–1950 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1511.02306) · [SIAM J. Comput.](https://doi.org/10.1137/16M1087072)  
> **Tags:** #QLSA #LCU #chebyshev #fourier #precision-scaling #linear-systems #variable-time-amplitude-amplification

---

## The computational problem

The **Quantum Linear Systems Problem (QLSP):** given a $d$-sparse $N \times N$ Hermitian matrix $A$ with condition number $\kappa$ and $\|A\| = 1$, plus a procedure $P_B$ to prepare $|b\rangle$, output a state $|\tilde{x}\rangle$ such that $\||\tilde{x}\rangle - |x\rangle\| \le \varepsilon$, where $|x\rangle \propto A^{-1}|b\rangle$.

Access to $A$ is via a sparse oracle $P_A$: (i) given column $j$ and index $\ell \in [d]$, return the row index of the $\ell$th nonzero entry; (ii) given $(j, k)$, return $A_{jk}$.

This is the same problem as in [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]], but the complexity measure is different: here the target is $\mathrm{poly}(\log(1/\varepsilon))$ rather than $\mathrm{poly}(1/\varepsilon)$.

## What the paper does

Removes the polynomial dependence on $1/\varepsilon$ from the [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL algorithm]] and replaces it with **polylogarithmic dependence** on $1/\varepsilon$. This is an exponential improvement in precision scaling, and it matters.

The key insight: instead of using [[Amplitude Estimation via Phase Estimation on Q|phase estimation]] to extract eigenvalues and then invert them (which costs $\Omega(1/\varepsilon)$ per call), represent $A^{-1}$ directly as a [[Linear Combination of Unitaries (LCU)|linear combination of unitaries]] — either $e^{-iAt_j}$ (Fourier approach) or Chebyshev polynomials $T_n(A/d)$ (Chebyshev approach). The precision scaling then comes from approximation theory rather than eigenvalue resolution, and approximation theory is much kinder here.

This is one of the papers where the field starts to move from "HHL + phase estimation" toward "matrix functions via approximation theory." That intellectual shift is the real contribution — it opens the door to [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] and all its descendants.

## The algorithm / construction

### General LCU framework (Section 2)

The starting point is the [[Linear Combination of Unitaries (LCU)|LCU lemma]]. Given $M = \sum_i \alpha_i U_i$ with $\alpha_i > 0$, define:
- **SELECT:** $U := \sum_i |i\rangle\langle i| \otimes U_i$  
- **PREPARE:** $V|0^m\rangle := \frac{1}{\sqrt{\alpha}} \sum_i \sqrt{\alpha_i}|i\rangle$, where $\alpha = \sum_i \alpha_i$

Then $W = V^\dagger U V$ satisfies:

$$W|0^m\rangle|\psi\rangle = \frac{1}{\alpha}|0^m\rangle M|\psi\rangle + |\Psi^\perp\rangle$$

Measuring and obtaining $|0^m\rangle$ gives $M|\psi\rangle / \|M|\psi\rangle\|$ with probability $(\|M|\psi\rangle\|/\alpha)^2$. [[Standard Amplitude Amplification|Amplitude amplification]] then boosts this to $O(\alpha / \|M|b\rangle\|)$ repetitions.

**Non-unitary extension (Lemma 7):** works when each $T_i$ is a *block* of a unitary $U_i$, not necessarily unitary itself. This is essential for the Chebyshev approach.

### Fourier approach (Section 3)

Represent $1/x$ as a linear combination of $e^{-ixt}$ for $x \in D_\kappa := [-1, -1/\kappa] \cup [1/\kappa, 1]$.

**Step 1 — Taming the singularity.** Since $1/x$ is unbounded near $0$, multiply by a bump function close to 1 on $D_\kappa$ and 0 near the origin. The choice is $f(y) = ye^{-y^2/2}$, which satisfies $\int_0^\infty f(y)\,dy = 1$ and is an eigenfunction of the Fourier transform.

**Step 2 — Integral representation.** Using $1/x = \int_0^\infty f(xy)\,dy$ and the Fourier transform of $f$:

$$\frac{1}{x} = \frac{i}{\sqrt{2\pi}} \int_0^\infty dy \int_{-\infty}^{\infty} dz\; z e^{-z^2/2} e^{-ixyz}$$

**Step 3 — Truncation and discretisation.** Truncate to finite ranges $y \in [0, y_J]$, $z \in [-z_K, z_K]$ with $y_J = \Theta(\kappa\sqrt{\log(\kappa/\varepsilon)})$ and $z_K = \Theta(\sqrt{\log(\kappa/\varepsilon)})$, then discretise with step sizes $\Delta y$ and $\Delta z$ to get:

$$h(x) = \frac{i}{\sqrt{2\pi}} \sum_{j=0}^{J-1} \Delta y \sum_{k=-K}^{K} \Delta z\; z_k e^{-z_k^2/2} e^{-ix y_j z_k}$$

Each term $e^{-iAy_j z_k}$ is a Hamiltonian simulation query. Poisson summation (Lemma 13) controls the discretisation error.

**Step 4 — Apply LCU + amplification.** The $\ell_1$ norm of coefficients is $\alpha = O(\kappa\sqrt{\log(\kappa/\varepsilon)})$. Implement via binary-decomposed SELECT (Lemma 8): since all unitaries are powers of $Y = e^{-iA\Delta y \Delta z}$, the conditional unitary $U = \sum_{j,k} |j,k\rangle\langle j,k| \otimes Y^{jk}$ decomposes into $O(\log(JK))$ controlled powers $c\text{-}Y^{2^r}$.

### Chebyshev approach (Section 4)

Replace $e^{-iAt}$ with Chebyshev polynomials $T_n(A/d)$, implemented via a [[Quantum-Walk Realization of Chebyshev Matrix Polynomials|quantum walk]].

**Step 1 — Quantum walk.** For $d$-sparse $A$ with $\|A\|_{\max} \le 1$, define the walk operator $W := S(2TT^\dagger - I)$ where $T$ is an isometry encoding the matrix structure. Within the invariant subspace of eigenvector $|\lambda\rangle$ of $H = A/d$:

$$W = \begin{pmatrix} \lambda & -\sqrt{1-\lambda^2} \\ \sqrt{1-\lambda^2} & \lambda \end{pmatrix}$$

and $W^n T|\lambda\rangle = T_n(\lambda) T|\lambda\rangle + \sqrt{1-\lambda^2}\, U_{n-1}(\lambda)|\perp_\lambda\rangle$. The first block gives $T_n(H)$ acting on $|\psi\rangle$.

**Step 2 — Chebyshev expansion of $1/x$.** Approximate $1/x$ by:

$$f(x) = \frac{1 - (1-x^2)^b}{x} = 4\sum_{j=0}^{b-1} (-1)^j \left[\frac{\sum_{i=j+1}^{b} \binom{2b}{b+i}}{2^{2b}}\right] T_{2j+1}(x)$$

for $b = \kappa^2 \log(\kappa/\varepsilon)$. The factor $(1-(1-x^2)^b)$ is the [[Chebyshev-Regularized Inverse Approximation|taming function]] — it kills the singularity at $x=0$ while being $\varepsilon$-close to 1 on $D_{\kappa d}$. Higher-order Chebyshev coefficients are exponentially suppressed (they're tail probabilities of binomial distributions — a Chernoff bound gives $e^{-j^2/b}$). Truncating at $j_0 = \sqrt{b\log(4b/\varepsilon)}$ preserves $\varepsilon$-accuracy.

**Step 3 — LCU + amplification.** The $\ell_1$ norm is $\alpha = O(\kappa \log(d\kappa/\varepsilon))$. Each term costs $O(n)$ walk steps; the maximum order is $O(d\kappa\log(d\kappa/\varepsilon))$.

### Variable-time amplitude amplification for near-linear $\kappa$ (Section 5)

Both approaches above give $O(d\kappa^2)$ query complexity. To reduce to near-linear $\kappa$, they combine:

1. **Gapped phase estimation** — a low-precision variant that distinguishes $|\theta| \le \phi$ from $|\theta| \ge 2\phi$ using $O(\frac{1}{\phi}\log\frac{1}{\varepsilon})$ queries. Buckets eigenvalues into $m = \lceil\log_2 \kappa\rceil + 1$ geometrically increasing ranges $[\phi_j, 2\phi_j]$ with $\phi_j = 2^{-j}$.

2. **Variable-stopping algorithm** $\mathcal{A} = \mathcal{A}_m \cdots \mathcal{A}_1$: step $j$ uses GPE to check if $|\lambda|$ falls in the $j$th bucket, and if so, applies $W(\phi_j, m\delta)$ — a version of $A^{-1}$ optimised for that eigenvalue range.

3. **[[Variable-Time Amplification for Condition-Number Reduction|Variable-time amplitude amplification]]** (Ambainis): the root-mean-square stopping time $t_{\text{avg}}$ is much less than $t_m$ because high-eigenvalue branches are cheap and common.

4. **Uncomputation** via algorithm $(\mathcal{A}')^\dagger$ — same as $\mathcal{A}$ but with identity replacing $W$ — to erase the ancillary GPE states. This works because the ancillary states depend only on GPE, not on the inversion.

## Key results

**Theorem 3 (Fourier approach):**

$$O\!\left(d\kappa^2 \log^{2.5}(\kappa/\varepsilon)\right) \text{ queries to } P_A, \quad O\!\left(\kappa\sqrt{\log(\kappa/\varepsilon)}\right) \text{ uses of } P_B$$

Gate complexity: $O(d\kappa^2 \log^{2.5}(\kappa/\varepsilon)(\log N + \log^{2.5}(\kappa/\varepsilon)))$.

**Theorem 4 (Chebyshev approach):**

$$O\!\left(d\kappa^2 \log^{2}(d\kappa/\varepsilon)\right) \text{ queries to } P_A, \quad O\!\left(\kappa\log(d\kappa/\varepsilon)\right) \text{ uses of } P_B$$

Gate complexity: $O(d\kappa^2 \log^{2}(d\kappa/\varepsilon)(\log N + \log^{2.5}(d\kappa/\varepsilon)))$.

**Theorem 5 (Linear $\kappa$ scaling):**

$$\tilde{O}(d\kappa\,\mathrm{poly}(\log(d\kappa/\varepsilon))) \text{ queries to } P_A \text{ and } P_B$$

This is near-optimal since $\kappa$-dependence cannot be sublinear.

| Algorithm | Query complexity | $\varepsilon$-dependence | $\kappa$-dependence |
|---|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | $O(d\kappa^2/\varepsilon \cdot \mathrm{polylog})$ | $\mathrm{poly}(1/\varepsilon)$ | $O(\kappa^2)$ |
| Fourier (this paper) | $O(d\kappa^2 \log^{2.5}(\kappa/\varepsilon))$ | $\mathrm{polylog}(1/\varepsilon)$ | $O(\kappa^2)$ |
| Chebyshev (this paper) | $O(d\kappa^2 \log^{2}(d\kappa/\varepsilon))$ | $\mathrm{polylog}(1/\varepsilon)$ | $O(\kappa^2)$ |
| VTAA (this paper) | $\tilde{O}(d\kappa\,\mathrm{polylog})$ | $\mathrm{polylog}(1/\varepsilon)$ | $\tilde{O}(\kappa)$ |
| [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes\|Costa et al. (2022)]] | $O(\kappa\,\mathrm{polylog})$ | $\mathrm{polylog}(1/\varepsilon)$ | $O(\kappa)$ |

## Comparison with prior work

The Fourier approach is more general: works whenever $A$ can be efficiently simulated, even if not sparse. The Chebyshev approach is tighter on $\kappa$ and $\varepsilon$ but requires sparse access. Both exponentially improve HHL's precision scaling.

The Fourier approach was later used to estimate hitting times of Markov chains (Chowdhury–Somma 2016) and to establish QMA results with exponentially small gaps (Fefferman–Lin 2016).

Later, [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] unified and superseded both approaches conceptually — applying arbitrary bounded polynomial transformations of singular values via a single clean framework. But the ideas here (function approximation ↔ LCU, taming unbounded functions, Chebyshev truncation) remain the conceptual ancestors.

## Limits / caveats

- Still outputs a **quantum state**, not the classical solution vector. Extracting $N$ classical amplitudes costs $O(N)$ additional work, negating the exponential speedup. For the expectation-value version, $\mathrm{polylog}(1/\varepsilon)$ is impossible unless BQP = PP.
- Requires $\|A\| = 1$ and known $\kappa$. In practice, estimating $\kappa$ can itself be expensive.
- Sparse oracle access is needed for the Chebyshev approach. The Fourier approach only needs Hamiltonian simulation, but still assumes efficient simulation.
- The VTAA construction (Section 5) is complex: gapped phase estimation + variable-stopping + uncomputation. Later optimal QLSA constructions ([[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al.]], [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin]]) are cleaner.

## Reusable ideas

1. [[Fourier Integral LCU for Matrix Inversion]] — represent $1/x$ via double integral of oscillatory functions, tame the singularity with a Gaussian-derived bump, discretise to get an LCU.
2. [[Chebyshev-Regularized Inverse Approximation]] — multiply $1/x$ by $(1-(1-x^2)^b)$ to tame the singularity, then exploit the exact Chebyshev expansion with exponentially decaying coefficients.
3. [[Quantum-Walk Realization of Chebyshev Matrix Polynomials]] — the quantum walk operator $W^n$ directly gives $T_n(H)$ in its top-left block, providing a native implementation of Chebyshev polynomials.
4. [[Variable-Time Amplification for Condition-Number Reduction]] — bucket eigenvalues geometrically, apply eigenvalue-range-specific inversions with different costs, then use VTAA to pay the RMS cost instead of the worst-case cost.
5. [[Gapped Phase Estimation for Eigenvalue Bucketing]] — low-precision phase estimation distinguishing $|\theta| \le \phi$ from $|\theta| \ge 2\phi$ with $O(\frac{1}{\phi}\log\frac{1}{\varepsilon})$ queries. Enables eigenvalue bucketing without full resolution.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim & Lloyd (2009)]] — HHL algorithm; this paper exponentially improves its precision dependence
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]] framework underlying both approaches
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry & Childs (2012)]] — quantum walk for [[Hamiltonian simulation]]
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes|Berry, Childs & Kothari (2015)]] — Hamiltonian simulation used for query complexity bounds
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. (2015)]] — truncated Taylor series; provides the simulation subroutine
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry et al. (2014)]] — exponential precision improvement for Hamiltonian simulation
- Ambainis (2012, arXiv:1010.4458) — variable-time amplitude amplification; adapted here with care to preserve polylog precision
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — connection between continuous and discrete quantum walks
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]]
- Sachdeva & Vishnoi (2013) — approximation theory framework for function decomposition; related classical techniques

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — predecessor; this paper fixes its precision bottleneck
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — LCU framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — later unification via polynomial transformations
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — achieves optimal $O(\kappa)$ with polylog precision
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — alternative route to polylog precision via adiabatic scheduling
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]] — uses this QLSA as a subroutine
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]] — applies this QLSA to PDEs with spectral and finite difference discretisations
- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]] — uses the high-precision QLSA framework for real-space simulation
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — downstream; uses QLSA in Carleman pipeline

### Trick cards
- [[Fourier Integral LCU for Matrix Inversion]]
- [[Chebyshev-Regularized Inverse Approximation]]
- [[Quantum-Walk Realization of Chebyshev Matrix Polynomials]]
- [[Variable-Time Amplification for Condition-Number Reduction]]
- [[Gapped Phase Estimation for Eigenvalue Bucketing]]
- [[Linear Combination of Unitaries (LCU)]]
