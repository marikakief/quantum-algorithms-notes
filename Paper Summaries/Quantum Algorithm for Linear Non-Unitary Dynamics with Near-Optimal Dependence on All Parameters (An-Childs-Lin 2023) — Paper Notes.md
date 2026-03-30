> **Source:** Dong An, Andrew M. Childs, and Lin Lin, *Quantum algorithm for linear non-unitary dynamics with near-optimal dependence on all parameters*, arXiv:2312.03916, Commun. Math. Phys. **407**(1), 19 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2312.03916) · [CMP](https://doi.org/10.1007/s00220-025-05509-w)
> **Tags:** #quantum-algorithm #non-unitary #LCHS #differential-equations #hamiltonian-simulation #kernel #Hardy-space #LCU #quadrature

---

## The computational problem

Solve a general linear ODE on a quantum computer:

$$\frac{\mathrm{d}}{\mathrm{d}t} u(t) = -A(t)\, u(t) + b(t), \quad u(0) = u_0, \quad t \in [0, T],$$

where $A(t) \in \mathbb{C}^{N \times N}$ has positive Hermitian part $L(t) = (A(t) + A(t)^\dagger)/2 \succeq 0$. The goal is to prepare a quantum state $\varepsilon$-close to the normalised solution $u(T)/\|u(T)\|$, given oracle access to $A(t)$ (via block-encoding or HAM-T oracle) and state-preparation oracles for $u_0$ and $b(t)$.

The two key cost measures are: (1) **matrix queries** — calls to the block-encoding or [[Hamiltonian simulation]] oracle for $A(t)$; (2) **state-preparation queries** — calls to the oracles preparing $|u_0\rangle$ and $|b(t)\rangle$.

---

## What the paper does

Introduces a generalised family of [[LCHS Kernel for Non-Unitary Dynamics|LCHS identities]] that express a non-unitary propagator $\mathcal{T} e^{-\int_0^T A(s)\, ds}$ as a continuous [[Linear Combination of Unitaries (LCU)|LCU]] of [[Hamiltonian simulation]] unitaries, with a tuneable kernel $f(k)$ replacing the original Cauchy kernel. By choosing a [[Near-Optimal Hardy-Space Kernel for LCHS|near-optimal kernel]] from a Hardy-space family, the precision dependence of the matrix-query cost drops from $O(1/\varepsilon)$ (original LCHS) to polylogarithmic in $1/\varepsilon$ — an exponential improvement.

This is the first quantum algorithm for linear ODEs achieving **both** optimal state-preparation cost $O((\|u_0\| + \|b\|_{L^1})/\|u(T)\|)$ **and** near-optimal matrix-query scaling on all parameters simultaneously.

**My assessment:** This is a significant result. The original LCHS (An-Liu-Lin, PRL 2023, arXiv:2303.01029) was a clever idea but had a fatal flaw in practice: $O(1/\varepsilon)$ precision scaling. This paper fixes that completely. The mathematical core — using Hardy-space kernels with sub-exponential decay, and proving via [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels|Phragmén–Lindelöf]] that you can't do better — is clean and satisfying. The result now sits alongside [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|time-marching]] and [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|Lap-LCHS]] as one of the main approaches to quantum ODE solving, and it feeds directly into the Lap-LCHS framework for general eigenvalue transforms.

---

## The algorithm / construction

### Step 1: Cartesian decomposition

Write $A(t) = L(t) + i H(t)$ where $L(t) = (A + A^\dagger)/2$ (Hermitian, assumed $\succeq 0$) and $H(t) = (A - A^\dagger)/(2i)$ (Hermitian).

### Step 2: Generalised LCHS identity (Theorem 6)

For any kernel $f(z)$ that is:
1. analytic on the open lower half-plane $\{z : \mathrm{Im}(z) < 0\}$ and continuous on $\{z : \mathrm{Im}(z) \leq 0\}$,
2. satisfies a decay bound $|z|^\alpha |f(z)| \leq e^C$ for some $\alpha > 0$ on $\mathrm{Im}(z) \leq 0$,
3. normalised: $\int_{\mathbb{R}} f(k)/(1 - ik)\, dk = 1$,

the identity holds:

$$\mathcal{T} e^{-\int_0^t A(s)\, ds} = \int_{\mathbb{R}} \frac{f(k)}{1 - ik}\, \mathcal{T} e^{-i \int_0^t [k L(s) + H(s)]\, ds}\, dk.$$

The original LCHS (arXiv:2303.01029) is the special case $f(k) = 1/(\pi(1 + ik))$, which gives the Cauchy kernel $1/(\pi(1+k^2))$ — only polynomial decay.

**Proof idea:** Lemma 7 shows the Cauchy principal value integral of $f(k) \cdot \mathcal{T} e^{-i\int(kL+H)}$ vanishes (by contour integration in the lower half-plane, using analyticity of $f$ and the exponential decay $e^{-k \int L}$ for $k < 0$ when $L \succ 0$). Dividing by $(1-ik)$ and using the normalisation condition recovers the propagator via residue at $k = -i$.

### Step 3: Near-optimal kernel choice

Choose the kernel family:

$$f_\beta(k) = \frac{1}{C_\beta}\, e^{(1 + ik)^\beta}, \quad C_\beta = 2\pi\, e^{-2^\beta}, \quad \beta \in (0,1).$$

On the real axis, $|f_\beta(k)| \sim e^{-c|k|^\beta}$ — sub-exponential decay, faster than any polynomial. This is the [[Near-Optimal Hardy-Space Kernel for LCHS|near-optimal Hardy-space kernel]].

### Step 4: Why you can't do better (Proposition 8)

A kernel satisfying the analyticity requirements of Theorem 6 **cannot** have true exponential decay $|f(k)| \leq C' e^{-c|k|}$ on $\mathbb{R}$ — such a function would necessarily be identically zero. The proof uses a [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels|Phragmén–Lindelöf principle]] (Lemma 9). So the $e^{-c|k|^\beta}$ family with $\beta < 1$ is asymptotically the best possible.

### Step 5: Discretisation via composite Gaussian quadrature

Truncate the $k$-integral to $[-K, K]$ with $K = O((\log 1/\varepsilon)^{1/\beta})$ (Lemma 10). Partition $[-K, K]$ into intervals of width $h_1 \sim 1/(e\, T \max_t \|L(t)\|)$ and apply $Q$-point Gaussian quadrature on each interval, with $Q = O(\log(1/\varepsilon))$ (Lemma 11). This gives:

$$\mathcal{T} e^{-\int A} \approx \sum_{j=0}^{M-1} c_j\, U(T, k_j), \quad M = O\!\left(T \max_t \|L(t)\| \cdot (\log 1/\varepsilon)^{1+1/\beta}\right),$$

where $U(T, k) = \mathcal{T} e^{-i\int_0^T [kL(s) + H(s)]\, ds}$ is a unitary [[Hamiltonian simulation]].

The coefficients satisfy $\sum_j |c_j| = O(1)$ (Lemma 14), so the [[Linear Combination of Unitaries (LCU)|LCU]] normalisation stays bounded.

### Step 6: Quantum implementation

1. **PREPARE:** Encode the quadrature weights $\{c_j\}$ into an ancilla state.
2. **SELECT:** For each $j$, implement $U(T, k_j)$ — [[Hamiltonian simulation]] of $k_j L(s) + H(s)$ for time $T$. Use [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-based simulation for time-independent $A$, or [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|truncated Dyson series]] for time-dependent $A$.
3. **LCU + amplitude amplification:** Standard [[Linear Combination of Unitaries (LCU)|LCU]] protocol gives the result with success probability $\Omega(\|u(T)\|^2 / (\|u_0\| + \|b\|_{L^1})^2)$; [[Standard Amplitude Amplification|amplitude amplification]] boosts this.

For the inhomogeneous term $\int_0^T \mathcal{T} e^{-\int_s^T A} \cdot b(s)\, ds$, discretise both the $k$-integral and the $s$-integral by composite Gaussian quadrature, yielding a [[Double-Integral Quadrature for Operator LCU|double-integral quadrature]].

---

## Key results

### Theorem 15 (main result, time-dependent)

The improved LCHS algorithm solves the linear ODE up to error $\varepsilon$ using:

$$\tilde{O}\!\left(\frac{\|u_0\| + \|b\|_{L^1}}{\|u(T)\|} \cdot \alpha_A T \cdot (\log 1/\varepsilon)^{1+1/\beta}\right) \text{ matrix queries}$$

and

$$O\!\left(\frac{\|u_0\| + \|b\|_{L^1}}{\|u(T)\|}\right) \text{ state-preparation queries},$$

where $\alpha_A \geq \max_t \|A(t)\|$ and $\beta \in (0,1)$ is the kernel parameter.

### Corollary 17 (time-independent, homogeneous)

When $A$ is time-independent and $b = 0$, using [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] for [[Hamiltonian simulation]] removes one $\log$ factor:

$$\tilde{O}\!\left(\frac{\|u_0\|}{\|u(T)\|} \cdot \alpha_A T \cdot (\log 1/\varepsilon)^{1/\beta}\right) \text{ matrix queries}.$$

### Corollary 18 (Gibbs state preparation)

For $L \succeq 0$, prepare the purified Gibbs state $e^{-\gamma L}/Z_\gamma$ using:

$$\tilde{O}\!\left(q_N\, Z_\gamma^{-1}\, \gamma\, \alpha_L \cdot (\log 1/\varepsilon)^{1/\beta}\right) \text{ queries to a block-encoding of } L.$$

### Corollary 19 (adaptive $\beta$)

Choosing $\beta = 1 - O(1/\log\log(1/\varepsilon))$ gives matrix-query complexity:

$$\tilde{O}\!\left(\frac{\|u_0\| + \|b\|_{L^1}}{\|u(T)\|} \cdot \alpha_A T \cdot (\log 1/\varepsilon)^2\right),$$

at the cost of increasing state-preparation queries by a $\log\log(1/\varepsilon)$ factor. This is the [[Adaptive Kernel Parameter for LCHS|adaptive parameter choice]].

---

## Comparison with prior work

| Method | Matrix queries | State-prep queries | Notes |
|---|---|---|---|
| Spectral [Berry 2014] | $\tilde{O}(\kappa_V \alpha_A T \cdot \mathrm{polylog}(1/\varepsilon) \cdot \|u_0\|/\|u(T)\|)$ | Same | Requires diagonalisability |
| Truncated Dyson [Berry+ 2017] | $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^2 \cdot \|u_0\|/\|u(T)\|)$ | Same | Near-optimal matrix queries but suboptimal state-prep |
| [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes\|Time-marching]] [Fang-Lin-Tong 2023] | $\tilde{O}(\alpha_A^2 T^2 \cdot \log(1/\varepsilon) \cdot \|u_0\|/\|u(T)\|)$ | $O(\alpha_A T \cdot \log(1/\varepsilon) \cdot \|u_0\|/\|u(T)\|)$ | Optimal state-prep cost but $T^2$ scaling |
| Original LCHS [An-Liu-Lin PRL 2023] | $\tilde{O}(\alpha_A T / \varepsilon \cdot \|u_0\|/\|u(T)\|)$ | $O(\|u_0\|/\|u(T)\|)$ | Optimal state-prep but $1/\varepsilon$ matrix queries |
| **This paper (time-dep.)** | $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^{1+1/\beta} \cdot \|u_0\|/\|u(T)\|)$ | $O(\|u_0\|/\|u(T)\|)$ | **Both optimal** |
| **This paper (time-indep.)** | $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^{1/\beta} \cdot \|u_0\|/\|u(T)\|)$ | $O(\|u_0\|/\|u(T)\|)$ | Best known |

The truncated Dyson method achieves $(\log 1/\varepsilon)^2$ matrix-query scaling but has suboptimal state-preparation cost. The original LCHS achieves optimal state-preparation cost but $O(1/\varepsilon)$ matrix-query scaling (because the Cauchy kernel decays too slowly). This paper gets both right simultaneously.

---

## Limits / caveats

1. **Dissipative assumption:** Requires $L(t) \succeq 0$ (the Hermitian part of $A(t)$ is positive semidefinite). Without this, the LCHS identity fails — the contour integral doesn't converge.

2. **$T^2$ for time-dependent case:** The time-dependent version has $\alpha_A T$ in the matrix-query cost, but the time-marching approach replaces $\alpha_A T$ with $\alpha_A^2 T^2$ while getting $\log(1/\varepsilon)$ precision. The improved LCHS trades better precision scaling for potentially better $T$-scaling, but neither achieves the conjectured optimal $O(\alpha_A T \cdot \mathrm{polylog}(1/\varepsilon))$ for time-dependent problems. The paper notes this remains open (see Section 4.4.4).

3. **Kernel choice:** The $\beta$-family is near-optimal but not unique. Other Hardy-space kernels satisfying Theorem 6 could work. The $\beta \to 1$ limit gives increasingly good decay but worsening constants ($\|f_\beta\|_{L^1}$ grows).

4. **Composite Gaussian quadrature constants:** While asymptotically near-optimal, the constants in the quadrature error bounds involve terms like $T \max_t \|L(t)\|$, which matter in practice.

5. **Section 4.4.4 (newly added):** Discusses open questions around whether the $(\log 1/\varepsilon)^{1+1/\beta}$ factor for time-dependent problems can be tightened to $(\log 1/\varepsilon)^{1/\beta}$ matching the time-independent case. The extra $\log$ comes from having to use time-dependent [[Hamiltonian simulation]] (truncated Dyson series) rather than QSP.

---

## Reusable ideas

1. **[[Near-Optimal Hardy-Space Kernel for LCHS]]** — the kernel family $f_\beta(k) = (1/C_\beta) e^{(1+ik)^\beta}$ with sub-exponential decay. This is the engine of the exponential precision improvement. Applicable whenever LCHS or [[Laplace-Transform LCU Lifting for Eigenvalue Transforms|Lap-LCHS]] is used.

2. **[[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]]** — the impossibility result (Proposition 8): an analytic kernel on the lower half-plane with normalisation cannot have true exponential decay. Sets a fundamental limit for any LCHS-type approach.

3. **[[Adaptive Kernel Parameter for LCHS]]** — choosing $\beta = 1 - O(1/\log\log(1/\varepsilon))$ trades a $\log\log$ factor in state-preparation cost for $(\log 1/\varepsilon)^2$ matrix-query scaling. Useful engineering trick for balancing the two cost measures.

4. **[[Generalised LCHS Identity]]** — the main theorem (Theorem 6): the LCHS identity holds for any kernel satisfying analyticity + decay + normalisation on the lower half-plane. This is strictly more general than the original Cauchy kernel version and opens the door to kernel optimisation.

---

## References within this paper

- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes|An-Liu-Lin (PRL 2023)]] — the original LCHS method this paper improves. arXiv:2303.01029.
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — time-marching approach. The main competitor for quantum ODE solving.
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] — truncated Dyson series, used as the [[Hamiltonian simulation]] subroutine for the time-dependent case.
- Berry, Childs, Ostrander, and Wang (2017) — QLSA-based ODE solver. arXiv:1701.05552. Not in vault.
- Childs, Kothari, and Somma (2017) — quantum spectral method. Not in vault.
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT, used for time-independent [[Hamiltonian simulation]] subroutine.
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — Lap-LCHS, the follow-up that extends this paper's kernel to general eigenvalue transforms.

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] — direct follow-up; uses the kernel from this paper
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] — competing approach to same problem
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — subroutine used here
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — subroutine for time-independent case
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] — related work by overlapping authors on time-dependent dynamics
- [[Quantum Linear Matrix Equations — Paper Notes]] — uses LCHS kernel

### Trick cards
- [[Near-Optimal Hardy-Space Kernel for LCHS]] — the kernel family introduced here
- [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]] — the impossibility result from Proposition 8
- [[Adaptive Kernel Parameter for LCHS]] — the $\beta$-optimisation trick
- [[Generalised LCHS Identity]] — the main identity (Theorem 6)
- [[LCHS Kernel for Non-Unitary Dynamics]] — the original LCHS idea this generalises
- [[Double-Integral Quadrature for Operator LCU]] — related discretisation technique
- [[Linear Combination of Unitaries (LCU)]] — underlying framework
