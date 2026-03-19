> **Source:** Dong An and Lin Lin, *Quantum Linear System Solver Based on Time-Optimal Adiabatic Quantum Computing and Quantum Approximate Optimization Algorithm*, arXiv:1909.05500, ACM Trans. Quantum Comput. **3**(2), Article 5 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/1909.05500) · [ACM TQC](https://doi.org/10.1145/3498331)
> **Tags:** #QLSA #adiabatic #QAOA #linear-systems #scheduling #time-optimal

---

## The computational problem

The **quantum linear system problem (QLSP)**: given oracle access to an invertible matrix $A \in \mathbb{C}^{N \times N}$ with condition number $\kappa = \|A\| \cdot \|A^{-1}\|$ (normalised so $\|A\| = 1$) and a state preparation oracle for $|b\rangle$, prepare a state $|x_a\rangle$ satisfying

$$\| |x_a\rangle\langle x_a| - |x\rangle\langle x| \|_2 \leq \varepsilon, \quad |x\rangle = \frac{A^{-1}|b\rangle}{\|A^{-1}|b\rangle\|}.$$

Lower bound on query complexity: $\Omega(\kappa)$ (from reduction to Grover search via Nayak and Wu). Any QLSA must be at least linear in $\kappa$.

## What the paper does

Shows that adiabatic quantum computing with an **optimally tuned scheduling function** solves QLSP in runtime $O(\kappa \cdot \text{poly}(\log(\kappa/\varepsilon)))$ — near-optimal in both $\kappa$ and $\varepsilon$. This is achieved through two scheduling strategies:

- **AQC(p)**: a power-law schedule giving $T = O(\kappa/\varepsilon)$ for $1 < p < 2$, linear in $\kappa$ but also linear in $1/\varepsilon$.
- **AQC(exp)**: a smooth (all-derivatives-vanish-at-endpoints) schedule giving $T = O(\kappa \log^2(\kappa) \cdot \log^4(\log \kappa / \varepsilon))$ — polylogarithmic in $1/\varepsilon$ at the cost of polylog factors in $\kappa$.

The paper also shows that QAOA with parameters derived from the optimal AQC schedule achieves the same asymptotic complexity, providing a variational route to the same result.

## The construction

### Adiabatic reformulation of QLSP

The key idea is to embed the linear system into an **eigenvalue-zero tracking problem** in a time-dependent Hamiltonian. Define:

$$H_0 = \sigma_x \otimes Q_b, \quad H_1 = \sigma_+ \otimes (AQ_b) + \sigma_- \otimes (Q_bA),$$

where $Q_b = I - |b\rangle\langle b|$ is the projector orthogonal to $|b\rangle$ and the system is doubled with an ancilla qubit (total dimension $2N$).

The interpolating Hamiltonian is $H(f(s)) = (1 - f(s))H_0 + f(s)H_1$, where $f: [0,1] \to [0,1]$ is a monotone scheduling function with $f(0) = 0$, $f(1) = 1$.

**The zero-eigenvalue subspace** of $H(f(s))$ always contains $|\bar{b}\rangle = |1, b\rangle$ and a state $|e_x(s)\rangle = |0, x(s)\rangle$ that smoothly interpolates from $|e_b\rangle = |0, b\rangle$ at $s = 0$ to $|e_x\rangle = |0, x\rangle$ at $s = 1$. The adiabatic theorem guarantees that evolving slowly enough tracks $|e_x(s)\rangle$ from the initial state $|e_b\rangle$ to the target $|e_x\rangle$.

The spectral gap satisfies $\Delta(f) \geq 1 - f + f/\kappa$, which closes to $O(1/\kappa)$ near $f = 1$ — this is where the $\kappa$-dependence enters.

This reformulation originates from Subası, Somma, and Orsucci (2019), building on the [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf]] idea of adapting adiabatic methods to specific problems.

### AQC(p): power-law gap-adapted scheduling

The schedule is defined implicitly by the ODE:

$$f'(s) = c_p \, \Delta_*(f(s))^p, \quad f(0) = 0,$$

where $\Delta_*(f) = 1 - f + f/\kappa$ is the gap lower bound and $c_p$ is a normalisation constant ensuring $f(1) = 1$. The idea is the same as [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf's local adiabatic evolution]]: slow down where the gap is small, speed up where it's large.

For $1 < p \leq 2$, this has a closed-form solution. The resulting evolution satisfies:

**Theorem 1 (HPD case):** For Hermitian positive definite $A$ with condition number $\kappa$ and $1 < p < 2$:

$$\| |{\psi_T(1)}\rangle\langle{\psi_T(1)}| - |e_x\rangle\langle e_x| \|_2 \leq \frac{C\kappa}{T}.$$

So $T = O(\kappa/\varepsilon)$ suffices. For $p = 1$ or $p = 2$, a $\log \kappa$ factor sneaks in: $T = O(\kappa \log \kappa / \varepsilon)$.

### AQC(exp): smooth scheduling for exponential precision

Uses a schedule where **all derivatives vanish at the endpoints**:

$$f(s) = \frac{1}{c_e} \int_0^s \exp\left(-\frac{1}{s'(1-s')}\right) ds', \quad c_e = \int_0^1 \exp\left(-\frac{1}{t(1-t)}\right) dt.$$

Because $H^{(k)}(0) = H^{(k)}(1) = 0$ for all $k \geq 1$, Nenciu-type superadiabatic analysis applies: the final-time error decays **exponentially** in a power of $T$.

**Theorem 2 (HPD case):**

$$\| |{\psi_T(1)}\rangle\langle{\psi_T(1)}| - |e_x\rangle\langle e_x| \|_2 \leq C \log \kappa \cdot \exp\!\left[-C'\left(\kappa \log^2 \kappa \cdot T\right)^{-1/4}\right].$$

This gives runtime $T = O(\kappa \log^2(\kappa) \cdot \log^4(\log \kappa / \varepsilon))$ — the $1/\varepsilon$ dependence is polylogarithmic. The schedule is **universal**: it doesn't require knowing $\kappa$ in advance.

### Extensions to general matrices

| Matrix class | Cost overhead |
|---|---|
| Hermitian positive definite | Baseline: Theorems 1–2 |
| General Hermitian | Factor 2 in qubit count (embed into HPD via $\sigma_z \otimes A + \sigma_x \otimes \kappa^{-1}I$) |
| General (non-Hermitian) | Standard dilation: $\hat{A} = \begin{pmatrix} 0 & A \\ A^\dagger & 0 \end{pmatrix}$; $\kappa$ doubles |

### QAOA formulation

The QAOA ansatz for QLSP is:

$$|\psi_\theta\rangle = e^{-i\gamma_P H_1} e^{-i\beta_P H_0} \cdots e^{-i\gamma_1 H_1} e^{-i\beta_1 H_0} |e_b\rangle,$$

with $P$ layers and angles $\{\beta_j, \gamma_j\}$. The connection to AQC: if you time-slice the AQC evolution into $P$ segments via first-order Trotterisation, the QAOA angles are:

$$\beta_j = \frac{(1 - f(s_j))}{T/P}, \quad \gamma_j = \frac{f(s_j)}{T/P}.$$

So AQC provides a principled initialisation for QAOA parameters. The paper's numerics suggest QAOA can beat AQC with the same $P$ because the variational optimisation finds better angles than the Trotter-discretised AQC schedule.

**Theorem 3 (QAOA complexity):** Using AQC(exp) parameters as initial guess, QAOA solves QLSP with the same asymptotic gate complexity as AQC(exp).

## Key results

| Method | Runtime $T$ | Query complexity (sparse-oracle, $d$-sparse $A$) | Precision dependence |
|---|---|---|---|
| AQC(p), $1 < p < 2$ | $O(\kappa/\varepsilon)$ | $\tilde{O}(d\kappa/\varepsilon)$ | Linear in $1/\varepsilon$ |
| AQC(exp) | $O(\kappa \log^2 \kappa \cdot \log^4(\log \kappa / \varepsilon))$ | $\tilde{O}(d\kappa \cdot \text{polylog}(d\kappa/\varepsilon))$ | Polylog in $1/\varepsilon$ |
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | — | $\tilde{O}(d^2 \kappa^2 / \varepsilon)$ | Poly in $1/\varepsilon$ (from QPE) |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes\|CKS (2017)]] | — | $\tilde{O}(d\kappa \cdot \text{polylog}(1/\varepsilon))$ | Polylog in $1/\varepsilon$ |
| Subası-Somma-Orsucci (2019) RM | $O(\kappa \log \kappa / \varepsilon)$ | — | Linear in $1/\varepsilon$ |

AQC(p) with $1 < p < 2$ removes the $\log \kappa$ factor from the randomisation method (RM) of Subası et al. AQC(exp) matches [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|CKS]] in $\kappa$ and $\varepsilon$ scaling without needing [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] or variable-time amplitude amplification.

## Comparison with prior work

The lineage is clear: [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] introduced gap-adapted scheduling for search; Subası-Somma-Orsucci (2019) applied adiabatic methods to QLSP with a randomisation trick achieving $O(\kappa \log \kappa / \varepsilon)$; this paper optimises the schedule to shave off the $\log \kappa$ and, with AQC(exp), gets polylogarithmic precision dependence.

The follow-up by Costa, An, Sanders, Su, Babbush, and Berry (2022, arXiv:2111.08152) uses a **discrete** adiabatic theorem to push the complexity to the optimal $O(\kappa \cdot \text{polylog}(\kappa/\varepsilon))$, removing the remaining polylog factors from AQC(exp).

The QAOA connection is interesting but somewhat preliminary: the theoretical guarantee just says QAOA is at least as good as time-sliced AQC. The numerics suggest it's better in practice, but there's no proof it achieves a fundamentally different scaling.

## Gate-level implementation

The time-dependent Hamiltonian $H(f(s))$ is simulated using the [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|truncated Dyson series]] method (or its interaction-picture variant from [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe 2018]]). The block-encodings of $A$ and $Q_b$ are constructed from sparse-oracle access to $A$ and a state-preparation oracle for $|b\rangle$.

- **Qubits:** $O(n + \log(d\kappa/\varepsilon))$ for AQC(p); $\tilde{O}(n + \log(d\kappa/\varepsilon))$ for AQC(exp).
- **Primitive gates:** $O(nd\kappa/\varepsilon \cdot \text{polylog}(d\kappa/\varepsilon))$ for AQC(p); $O(nd\kappa \cdot \text{polylog}(d\kappa/\varepsilon))$ for AQC(exp).

## Limits / caveats

- **Still produces a quantum state**, not a classical solution. The usual QLSA caveat applies: the exponential speedup is only meaningful if the downstream computation can use $|x\rangle$ directly.
- **AQC(exp) has worse $\kappa$-dependence** than AQC(p): the polylog factors in $\kappa$ are the price for polylogarithmic precision. For moderate $\varepsilon$, AQC(p) may win.
- **The QAOA guarantee is weak**: it only matches AQC, not beats it. The numerical evidence for QAOA outperforming AQC is encouraging but limited to small instances.
- **Superseded in asymptotics** by Costa et al. (2022), which achieves optimal $O(\kappa \cdot \text{polylog}(\kappa/\varepsilon))$ via a discrete adiabatic theorem. This paper's primary contribution is conceptual: it shows that scheduling optimisation alone gets you most of the way.
- **No explicit analysis of the constant factors** in the gate complexity. The Dyson series simulation introduces overhead that may be significant in practice.

## Reusable ideas

1. **[[Gap-Adapted Adiabatic Scheduling]]** — choosing the schedule $f(s)$ so the evolution slows near spectral gap minima, directly generalising [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf's local adiabatic method]] from search to linear systems (and potentially any eigenpath-tracking problem).

2. **[[Superadiabatic Smooth Scheduling for Exponential Precision]]** — using a schedule with all derivatives vanishing at endpoints to activate Nenciu-type superadiabatic bounds, trading polynomial $1/\varepsilon$ for polylogarithmic $1/\varepsilon$ at the cost of worse polylog factors in other parameters.

3. **[[AQC-to-QAOA Parameter Transfer]]** — initialising QAOA variational parameters from Trotter-discretised AQC schedules, providing a principled warm start that avoids the barren-plateau problem in QAOA parameter landscapes.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim, Lloyd (2009)]] — the original HHL algorithm; this paper improves upon its $O(\kappa^2/\varepsilon)$ scaling.
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs, Kothari, Somma (2017)]] — the CKS QLSA with polylog precision; AQC(exp) matches this scaling via a completely different route.
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland and Cerf (2002)]] — the local adiabatic method that directly inspired AQC(p).
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma and Boixo (2013)]] — spectral gap amplification, related adiabatic techniques.
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi, Goldstone, Gutmann (2014)]] — the original QAOA proposal.
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi, Goldstone, Gutmann, Sipser (2000)]] — the foundational AQC proposal.
- Subası, Somma, Orsucci (2019) — the randomisation method (RM) for adiabatic QLSP that AQC(p) improves upon. Not yet in the vault.
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer, Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians, used for gate implementation.
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low and Wiebe (2018)]] — interaction-picture Dyson series simulation.
- Nenciu (1993) — superadiabatic theory underlying AQC(exp). Not in the vault; classical adiabatic perturbation theory reference.
- Costa, An, Sanders, Su, Babbush, Berry (2022, arXiv:2111.08152) — follow-up achieving optimal QLSA scaling via discrete adiabatic theorem. Not yet in the vault.

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]

- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes]] — follow-up adding eigenstate filtering to make the AQC approach rigorous; also introduces Zeno alternative

### Trick cards
- [[Gap-Adapted Adiabatic Scheduling]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
- [[AQC-to-QAOA Parameter Transfer]]
- [[Chebyshev Polynomial Spectral Projector]]
