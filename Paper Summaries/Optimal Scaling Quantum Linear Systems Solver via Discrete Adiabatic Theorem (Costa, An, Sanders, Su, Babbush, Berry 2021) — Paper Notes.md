> **Source:** Pedro C. S. Costa, Dong An, Yuval R. Sanders, Yuan Su, Ryan Babbush, and Dominic W. Berry, *Optimal scaling quantum linear systems solver via discrete adiabatic theorem*, arXiv:2111.08152, PRX Quantum (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.08152) · [PDF](https://arxiv.org/pdf/2111.08152)
> **Tags:** #QLSP #linear-systems #adiabatic #qubitization #quantum-walk #optimal-scaling #block-encoding #eigenstate-filtering

---

## The computational problem

The **quantum linear system problem (QLSP):** given oracle access to a block encoding of an $N \times N$ matrix $A$ with $\|A\| = 1$ and $\|A^{-1}\| = \kappa$, and an oracle preparing $|b\rangle$, produce a quantum state $\varepsilon$-close to $|x\rangle = A^{-1}|b\rangle / \|A^{-1}|b\rangle\|$.

The relevant parameters are the condition number $\kappa$ and target precision $\varepsilon$. The known lower bound is $\Omega(\kappa \log(1/\varepsilon))$.

---

## What the paper does

This paper gives the first QLSP solver with **optimal** $O(\kappa \log(1/\varepsilon))$ query complexity, matching the lower bound. The key innovation is a **discrete adiabatic theorem** — a rigorous version of the Dranov-Kellendonk-Seiler (DKS) result that tracks the spectral gap dependence explicitly. Combined with [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and a new LCU-based eigenstate filtering method, this eliminates the $\log \kappa$ factor that plagued all prior approaches.

Worth noting: Yuval is a co-author, and the paper builds directly on techniques from [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]], particularly the [[Stroboscopic Adiabatic Walk]] idea — though the approach here is fundamentally different in that it proves exact gap-dependent error bounds rather than relying on heuristic analysis.

---

## The algorithm / construction

### Setup

For the QLSP $Ax = b$, define two Hamiltonians:

$$H_0 = \begin{pmatrix} 0 & Q_b \\ Q_b & 0 \end{pmatrix}, \quad H_1 = \begin{pmatrix} 0 & AQ_b \\ Q_b A & 0 \end{pmatrix}$$

where $Q_b = I - |b\rangle\langle b|$. The state $|0, b\rangle$ is an eigenstate of $H_0$ (eigenvalue 0) and adiabatic evolution carries it to the eigenstate $|0, A^{-1}b\rangle$ of $H_1$.

For general (non-Hermitian, non-positive-definite) $A$, the construction uses an enlarged space:

$$H_1 = \sigma_+ \otimes [\mathcal{A} Q_b] + \sigma_- \otimes [Q_b \mathcal{A}]$$

where $\mathcal{A} = \begin{pmatrix} 0 & A \\ A^\dagger & 0 \end{pmatrix}$, and the spectral gap is $\Delta_0(s) \geq \sqrt{(1 - f(s))^2 + (f(s)/\kappa)^2}$.

### Scheduling

Interpolate via $H(s) = (1 - f(s))H_0 + f(s)H_1$ with a schedule function:

$$f(s) = \frac{\kappa}{\kappa - 1}\left[1 - \left(1 + s(\kappa^{p-1} - 1)\right)^{1/(1-p)}\right]$$

for $1 < p < 2$ (the paper analyzes $p = 3/2$ in detail). This satisfies $\dot{f}(s) = d_p \Delta_0^p(s)$, slowing the evolution where the gap is smallest.

### Discrete adiabatic evolution

Rather than simulating $H(s)$ via Dyson series (which introduces a $\log \kappa$ overhead), they [[Qubitization (Quantum Walk for Spectral Encoding)|qubitize]] $H(s)$ to get walk operators $W_T(s)$ and run a discrete adiabatic evolution:

$$U_T(s) = \prod_{n=0}^{sT-1} W_T(n/T)$$

The [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]] bounds the error between $U_T(s)$ and the ideal adiabatic evolution $U_T^A(s)$. The main result (Theorem 3, simplified form):

$$\|U_T(s) - U_T^A(s)\| \leq \frac{12\hat{c}_1(0)}{T\check{\Delta}(0)^2} + \frac{12\hat{c}_1(s)}{T\check{\Delta}(s)^2} + \frac{6\hat{c}_1(s)}{T\check{\Delta}(s)} + \text{three sums with } O(1/T^2) \text{ terms}$$

where $\hat{c}_k(s)$ bounds the $k$th finite difference of $W_T(s)$ and $\check{\Delta}(s)$ is the spectral gap.

### Block encoding of $H(s)$

The $s$-dependence enters through a rotation $R(s)$ that mixes $H_0$ and $H_1$ contributions. Clever trick: instead of using $R(s)$ and $R(s)^\dagger$ symmetrically, they pair $R(s)$ with a Hadamard — this block-encodes $A(f(s)) / \sqrt{2[(1-f)^2 + f^2]}$ with a prefactor between $1/\sqrt{2}$ and $1$. The asymmetry is handled by swapping the order in the two blocks of $H(s)$, preserving the overall symmetry required for qubitization.

### Eigenstate filtering

After $T = O(\kappa)$ steps of the discrete adiabatic walk (to achieve constant-error overlap with the target), apply [[Dolph-Chebyshev Eigenstate Filtering|eigenstate filtering]] to boost precision to $\varepsilon$. This uses a linear combination of walk powers with [[Coherent Alias Sampling for PREPARE|Dolph-Chebyshev window]] weights:

$$\tilde{w}(\phi) = \varepsilon \, T_\ell(\beta \cos \phi)$$

where $T_\ell$ is the Chebyshev polynomial of order $\ell$ and $\beta = \cosh(\cosh^{-1}(1/\varepsilon)/\ell)$. Setting the width to match the gap gives:

$$\ell = \frac{\cosh^{-1}(1/\varepsilon)}{\cosh^{-1}(1/\cos(1/\kappa))} \leq \kappa \ln(2/\varepsilon)$$

The LCU is implemented using a **unary encoding with two-qubit recycling** (Figures 5–7 in the paper): each controlled walk step uses one ancilla qubit, prepared by a controlled rotation from the previous one, and measured immediately after use. This needs only **two ancilla qubits** at any time, compared to one for singular value processing — but with the enormous advantage that the rotation angles have a closed-form expression (no numerically demanding QSP angle-finding).

A bonus: sequential measurement means failures are detected early (on average halfway through), halving the expected cost of failed runs.

---

## Key results

**Theorem 19 (main result):** Given a block encoding of $A$ and an oracle for $|b\rangle$, the QLSP can be solved to precision $\varepsilon$ using

$$O(\kappa \log(1/\varepsilon))$$

oracle calls. This matches the lower bound $\Omega(\kappa \log(1/\varepsilon))$ of Harrow-Kothari.

**Theorem 3 (discrete adiabatic theorem):** For a sequence of walk operators $W_T(s)$ with bounded first and second differences, and $T \geq \max_s(4\hat{c}_1(s)/\check{\Delta}(s))$, the error is bounded by an explicit expression with leading term $O(1/T)$ and explicit gap dependence through $1/\check{\Delta}(s)^3$ (in the sums).

**Theorem 17 (constant factors, $p = 3/2$):**
- Positive-definite Hermitian $A$: $\|U_T - U_T^A\| \leq 5632\kappa/T + O(\sqrt{\kappa}/T)$
- General $A$: $\|U_T - U_T^A\| \leq 15307\kappa/T + O(\sqrt{\kappa}/T)$

Numerically, the actual scaling constant is $\sim 638\kappa/T$, about $9\times$ smaller than the analytical bound. This means ~$834\kappa$ walk steps suffice for 50% overlap (constant-error adiabatic step).

**Theorem 18 (general $p$):** For any $1 < p < 2$, the discrete adiabatic error scales as $C_p \cdot \kappa/T$, confirming linear-$\kappa$ scaling for the entire parameter family. The constant $C_p$ is loose (not optimized).

---

## Comparison with prior work

| Year | Authors | Approach | Query complexity |
|---|---|---|---|
| 2008 | Harrow, Hassidim, Lloyd (HHL) | Phase estimation + inversion | $O(\kappa^2/\varepsilon)$ |
| 2012 | Ambainis | Variable-time amplitude amplification | $O(\kappa(\log(\kappa)/\varepsilon)^3)$ |
| 2017 | Childs, Kothari, Somma (CKS) | Fourier/Chebyshev fitting via LCU | $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$ |
| 2018 | Subaşı, Somma, Orsucci | Adiabatic randomization | $O(\kappa \log \kappa / \varepsilon)$ |
| 2019 | An, Lin | Time-optimal adiabatic | $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$ |
| 2019 | Lin, Tong | Zeno eigenstate filtering | $O(\kappa \log(\kappa/\varepsilon))$ |
| **2021** | **Costa, An, Sanders, Su, Babbush, Berry** | **Discrete adiabatic + Dolph-Chebyshev filtering** | $O(\kappa \log(1/\varepsilon))$ **[optimal]** |

The progression is clear: HHL → variable-time AA → polynomial methods → continuous adiabatic → discrete adiabatic. Each step removed a log factor or polylog overhead. The continuous adiabatic approaches (Subaşı, An-Lin, Lin-Tong) all incur extra $\log \kappa$ factors because they use the Dyson series to simulate $H(s)$ — the discretization error is the bottleneck. By going directly to quantum walks, this paper sidesteps that entirely.

My assessment: this is the definitive QLSP algorithm for asymptotic scaling. The constant factors (~$834\kappa$ walk steps for the adiabatic part) are large but the authors identify them precisely, which is unusual and valuable for resource estimation. In practice the filtering step ($\kappa \ln(2/\varepsilon)$) dominates only for $\varepsilon < 10^{-180}$ or so — for realistic precisions the adiabatic step is the bottleneck due to the large constant.

**Update:** [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes|Costa, An, Babbush, Berry (2023)]] showed via numerical testing that the actual constant is ~1,500× smaller than the analytical bound ($\alpha \approx 1.56$ vs. $\alpha \approx 2305$). This makes the algorithm far more practical than the analytical bound suggests.

---

## Limits / caveats

1. **Constant factors are large.** The analytical bound on the adiabatic step gives $\sim 5632\kappa/T$ (Hermitian case), about $9\times$ the numerically observed $\sim 638\kappa/T$. The constant in the filtering step ($\kappa \ln(2/\varepsilon)$) is much smaller. The authors explicitly note that "over an order of magnitude" gap between analytical and numerical constants remains.

2. **Sparsity scaling is suboptimal.** For sparse matrices accessed via entry oracles, the lower bound includes a $\sqrt{d}$ factor in sparsity. Standard block-encoding methods are linear in $d$, so achieving $\sqrt{d}$ while maintaining strict linear-$\kappa$ is still open. One could use Low's nested interaction picture approach (Ref [9]) to get $\sqrt{d}$ up to logs, but this reintroduces $\log \kappa$ factors.

3. **Assumes a good block encoding.** The result counts queries to a block encoding oracle. The cost of constructing that block encoding from more fundamental access models (sparse matrix oracles, etc.) is separate and problem-dependent.

4. **The $\kappa \log(1/\varepsilon)$ complexity is multiplicative, not additive.** Unlike Hamiltonian simulation (where $t$ and $\log(1/\varepsilon)$ enter additively), here $\kappa$ and $\log(1/\varepsilon)$ must multiply. This is a fundamental feature of the problem, not a limitation of the algorithm.

5. **The scheduling parameter $p$ affects constants significantly.** Numerically, $p \approx 1.3$ minimizes the error bound (not the analytically convenient $p = 3/2$). The optimal $p$ may depend on $\kappa$.

---

## Reusable ideas

1. [[Discrete Adiabatic Theorem for Quantum Walks]] — Rigorous error bound for discrete-time adiabatic evolution with explicit gap dependence; avoids Dyson series overhead entirely.

2. [[Dolph-Chebyshev Eigenstate Filtering]] — LCU-based filtering using Chebyshev polynomials with closed-form weights; two ancilla qubits; early failure detection; same asymptotic efficiency as QSP-based filtering but much simpler to implement.

3. [[Asymmetric Block Encoding via Hadamard Pairing]] — Using $R(s)$ paired with Hadamard (instead of $R(s)^\dagger$) to simplify the $s$-dependent block encoding; overall symmetry preserved by swapping the order in the two blocks.

---

## References within this paper

- **Harrow, Hassidim, Lloyd (HHL) [1]** — Original QLSP algorithm with $O(\kappa^2/\varepsilon)$ complexity; the paper that started this line of work.
- **Ambainis [10]** — Variable-time amplitude amplification for QLSP; near-linear $\kappa$ but poor $\varepsilon$ scaling.
- **Childs, Kothari, Somma (CKS) [11]** — LCU-based QLSP solver with $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$; first to achieve $\log(1/\varepsilon)$ precision scaling.
- **Subaşı, Somma, Orsucci [14]** — First adiabatic approach to QLSP; near-linear $\kappa$ without variable-time AA.
- **An, Lin [15]** — Time-optimal adiabatic QLSP; improved to $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$.
- **Lin, Tong [16]** — Zeno eigenstate filtering; $O(\kappa \log(\kappa/\varepsilon))$; the immediate predecessor and the approach closest to optimal before this paper.
- **Harrow, Kothari [17]** — The $\Omega(\kappa \log(1/\varepsilon))$ lower bound (in preparation at time of writing).
- **Dranov, Kellendonk, Seiler (DKS) [18]** — Original discrete adiabatic theorem (1998); proved $O(1/T)$ error but without gap dependence.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Costa et al. [24]]] — [[Stroboscopic Adiabatic Walk]] for optimization; earlier (less rigorous) use of discrete qubitized walks for adiabatic evolution.
- Berry, Kieferová, Scherer, Sanders, Low, Wiebe, Gidney, Babbush [25] — npj QI 2018; techniques for Hamiltonian simulation that fed into the block encoding approach used here.

---

## Cross-links

### Paper notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Direct precursor: same author group, introduced [[Stroboscopic Adiabatic Walk]] using qubitized walks; this paper makes the approach rigorous and optimal for QLSP.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Introduces [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] infrastructure (PREPARE/SELECT, walk operator, [[QROM (Quantum Read-Only Memory)|QROM]], [[Unary Iteration]]) that this paper builds on.
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — direct predecessor: near-optimal $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$ via continuous adiabatic scheduling; this paper eliminates the residual $\log\kappa$ factors by switching to discrete qubitized walks
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the original HHL algorithm; this paper achieves the optimal scaling that HHL's $O(\kappa^2/\varepsilon)$ originally left open
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — concurrent preconditioning-based QLSP; complementary approach that handles structured matrices with classically-accessible eigenvalues
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — established the equivalence between adiabatic and circuit-model computation; the discrete adiabatic theorem here is a rigorous quantum-walk version of the continuous adiabatic theorem used there
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Cites this paper in the context of QLSP-based ODE approaches; argues that QLSP solvers (including the optimal one) are suboptimal for oscillator simulation due to encoding-dependent condition-number overhead.
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — numerical follow-on showing the practical constant is ~1,500× smaller than the analytical bound
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — older HHL-based ODE solver; this paper provides the optimal QLSP primitive that later ODE algorithms can plug in instead
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — improved linear ODE solver whose QLSA subroutine is superseded by the optimal discrete-adiabatic QLSP here
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — time-dependent ODE solver from overlapping authors; uses the improved QLSP lineage together with Dyson-series machinery
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — Dyson-series Hamiltonian simulation from the same broader Berry/Kieferová line; related block-encoding machinery feeds into later ODE solvers

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — The walk operator used here
- [[Stroboscopic Adiabatic Walk]] — Earlier, heuristic version of discrete adiabatic walks
- [[Coherent Alias Sampling for PREPARE]] — Used in the PREPARE oracle for the walk
- [[Discrete Adiabatic Theorem for Quantum Walks]] — New trick from this paper
- [[Dolph-Chebyshev Eigenstate Filtering]] — New trick from this paper
- [[Asymmetric Block Encoding via Hadamard Pairing]] — New trick from this paper

### Follow-up
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — Numerical validation showing the actual constant factor is ~1,500× smaller than the analytical bound.
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — Extends the discrete adiabatic framework to show time step sizes can be $\varepsilon$-independent for any-order product formulas; boundary cancellation gives super-polynomial convergence.
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — uses the optimal QLSP solver as subroutine for nonlinear ODEs after Carleman linearisation
