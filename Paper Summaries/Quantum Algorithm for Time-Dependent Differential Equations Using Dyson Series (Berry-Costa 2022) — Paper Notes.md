> **Source:** Dominic W. Berry and Pedro C. S. Costa, *Quantum algorithm for time-dependent differential equations using Dyson series*, arXiv:2212.03544, Quantum **8**, 1369 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2212.03544) · [Quantum](https://doi.org/10.22331/q-2024-06-13-1369)
> **Tags:** #quantum-algorithm #differential-equations #dyson-series #linear-systems #QLSA #block-encoding #time-dependent

---

## The computational problem

Solve the initial-value problem for an inhomogeneous linear ODE:

$$\dot{x}(t) = A(t)x(t) + b(t), \quad x(0) = x_0,$$

where $A(t) \in \mathbb{C}^{N \times N}$ is a time-dependent coefficient matrix, $b(t) \in \mathbb{C}^N$ is a driving term, and $x(t) \in \mathbb{C}^N$ is the solution vector. The goal is to prepare a quantum state whose amplitudes encode $x(T)$, with error $\varepsilon$ in the Bures–Wasserstein metric.

**Stability requirement:** The logarithmic norm $\mu(A(t)) = \lambda_{\max}([A(t) + A^\dagger(t)]/2)$ must be non-positive for all $t \in [0,T]$. This ensures dissipative dynamics and bounds the time-ordered exponential.

## What the paper does

Combines the [[History-State Linear System Encoding for ODE Trajectories|history-state linear system encoding]] from [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] with the [[Sorting-Based Time-Ordering for Dyson LCU|Dyson series block-encoding]] from [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] and [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]]. The result is the first quantum algorithm for *time-dependent* inhomogeneous linear ODEs with logarithmic dependence on precision, linear-in-$T$ oracle complexity (up to logs), and oracle-call count independent of derivatives of $A(t)$ and $b(t)$.

This is a genuine synthesis rather than an incremental extension. The block matrix from Berry-Childs-Ostrander-Wang encoded terms of the Taylor series in extra lines of the matrix. Here, each line encodes a *time step*, and the Dyson series is used to *block-encode* the propagator $V_m = W_K(m\Delta t, (m-1)\Delta t)$ for each segment. The switch from Taylor lines to Dyson block-encodings is what makes the time-dependent case tractable.

The [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|optimal QLSA (Costa et al. 2022)]] is used as the linear system solver, which is where the $O(\kappa \log(1/\varepsilon))$ scaling comes from.

## The algorithm / construction

### Step 1: Express the solution via Dyson series

The general solution decomposes as $x(t) = x_H(t) + x_P(t)$:

- **Homogeneous part:** $x_H(t) = W(t, t_0) x_0$ where $W(t,t_0)$ is the time-ordered exponential (Dyson series propagator)
- **Particular part:** $x_P(t) = v(t, t_0)$ where $v$ is a Dyson-like series with the final $A(t_k)$ replaced by $b(t_k)$

Both are truncated at order $K$ and computed on segments of length $\Delta t \approx 1/\lambda_A$.

### Step 2: Encode in a block-bidiagonal linear system

Divide $[0,T]$ into $r = \lceil \lambda_A T \rceil$ evolution segments, followed by $r$ padding segments (the [[History-State Padding for Final-Time Readout|padding trick]] to boost the measurement probability). The linear system is:

$$\mathcal{A}\vec{X} = \vec{B}$$

where $\mathcal{A}$ is a $2r \times 2r$ block matrix (each block $N \times N$):

$$\mathcal{A} = \begin{pmatrix} I & & \\ -V_1 & I & \\ & -V_2 & I & \\ & & \ddots & \ddots \\ & & & -V_r & I & \\ & & & & -I & I & \\ & & & & & \ddots & \ddots \end{pmatrix}$$

The first $r$ rows encode the Dyson series stepping $\tilde{x}(m\Delta t) = V_m \tilde{x}((m-1)\Delta t) + v_m$. The last $r$ rows freeze the solution at $x(T)$.

**Condition number:** $\kappa_{\mathcal{A}} = O(R)$ where $R = 2r$ is the total number of rows. The logarithmic-norm stability condition keeps $\|V_m\| \le 1 + \varepsilon$, so $\|\mathcal{A}^{-1}\| = O(R)$ and $\|\mathcal{A}\| = O(1)$.

### Step 3: Block-encode the linear system

The hard part. The matrix $\mathcal{A}$ decomposes as $I - S \cdot D$ where $S$ is an increment on the time register and $D$ is block-diagonal with $V_m$ on each block. Block-encoding $D$ requires block-encoding each truncated Dyson series:

$$V_m = W_K(m\Delta t, (m-1)\Delta t) = \sum_{k=0}^{K} \int_{t_0}^{t_0+\Delta t} dt_1 \int_{t_0}^{t_1} dt_2 \cdots \int_{t_0}^{t_{k-1}} dt_k \, A(t_1) \cdots A(t_k)$$

This uses the same [[Sorting-Based Time-Ordering for Dyson LCU|sorting-based time-ordering]] machinery from Kieferová-Scherer-Berry. The key steps:
1. Prepare $K$ time registers in equal superposition over $M$ discretization points
2. Sort via a reversible sorting network ($O(K \log K \log M)$ gates)
3. Apply $K$ controlled calls to the block-encoding of $A(t)$
4. Prepare a register encoding $k$ via controlled rotations

With $\lambda_A \Delta t \le 1$, the $\lambda$-value of the Dyson block-encoding is at most $e$.

The right-hand side $\vec{B}$ also needs block-encoding. The $v_m$ components use a similar Dyson construction with $K-1$ calls to $U_A$ and one call to $U_b$. Amplitude amplification boosts the preparation success probability.

### Step 4: Solve via optimal QLSA

Apply the [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2022)]] QLSA with $O(\kappa_{\mathcal{A}} \log(1/\varepsilon)) = O(\lambda_A T \log(1/\varepsilon))$ calls to block-encodings of $\mathcal{A}$ and $\vec{B}$.

### Step 5: Amplitude amplification for final-time readout

Measure the time register to obtain $x(T)$. The padding ensures amplitude $\Omega(\|x(T)\|/x_{\max})$, requiring $O(x_{\max}/\|x(T)\|)$ rounds of amplitude amplification.

## Key results

### Time-dependent case (Theorem 4.1)

$$O\!\left(R \lambda_A T \log(1/\varepsilon)\right) \text{ calls to } U_b, U_x$$

$$O\!\left(R \lambda_A T \log(1/\varepsilon) \cdot \log\!\frac{\lambda_{Ax} T}{\varepsilon}\right) \text{ calls to } U_A$$

$$O\!\left(R \lambda_A T \log(1/\varepsilon) \cdot \log\!\frac{\lambda_{Ax} T}{\varepsilon} \cdot \left[\log\!\frac{TD}{\lambda_A \varepsilon} + \log\!\frac{\lambda_A T}{\varepsilon}\right]\right) \text{ additional gates}$$

where:
- $\lambda_{Ax} = \max(\lambda_A, b_{\max}/x_{\max})$
- $R$ accounts for amplitude amplification (typically $O(1)$ unless the solution decays significantly or cancellations in $v_m$ occur)
- $D$ depends on first derivatives of $A(t)$ and $b(t)$ only (and only in the gate count, not oracle calls)

### Time-independent case (Theorem 4.2)

$$O\!\left(R \lambda_A T \log(1/\varepsilon)\right) \text{ calls to } U_b, U_x$$

$$O\!\left(R \lambda_A T \log(1/\varepsilon) \cdot \log\!\frac{\lambda_{Ax} T}{\varepsilon}\right) \text{ calls to } U_A$$

$$O\!\left(R \lambda_A T \log(1/\varepsilon) \cdot \log\!\frac{\lambda_{Ax} T}{\varepsilon} \cdot \log\!\frac{\lambda_A T}{\varepsilon}\right) \text{ additional gates}$$

with $R = O(x_{\max}/\|x(T)\|)$ — no amplitude amplification needed for state preparation, and no derivative dependence at all.

## Comparison with prior work

| Method | Oracle calls ($U_A$) | Calls to $U_b$, $U_x$ | Derivative dependence | Time-dependent? | Inhomogeneous? |
|---|---|---|---|---|---|
| [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes\|Berry (2014)]] | $\tilde{O}(\lambda_A^2 T^2/\varepsilon^2)$ | Same | None | Yes | Yes |
| [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes\|Berry-Childs-Ostrander-Wang (2017)]] | $\tilde{O}(\lambda_A T \cdot \text{polylog}(1/\varepsilon))$ | Same | None | No (constant $A$) | Yes |
| Childs-Liu (2020) spectral method | $\tilde{O}(\lambda_A T \cdot \log^4(T/\varepsilon) \log(1/\varepsilon))$ | Same scaling | Arbitrary-order derivatives ($g'$) | Yes | Yes |
| [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes\|Fang-Lin-Tong (2023)]] | $\tilde{O}(\lambda_A^2 T^2)$ | $O(1)$ per step | Bounded variation | Yes | No (homogeneous only) |
| **This paper** | $\tilde{O}(\lambda_A T \cdot \log(\lambda_{Ax} T/\varepsilon) \log(1/\varepsilon))$ | $\tilde{O}(\lambda_A T \log(1/\varepsilon))$ | 1st derivatives (gates only) | **Yes** | **Yes** |

The main improvements over the spectral method of Childs-Liu (2020):
- Oracle calls: $\log(T/\varepsilon) \log(1/\varepsilon)$ vs. $\log^4(T/\varepsilon) \log(1/\varepsilon)$ — a $\log^3$ factor improvement
- $U_b$ calls: $\log(1/\varepsilon)$ vs. $\log^4(T/\varepsilon) \log(1/\varepsilon)$ — much better because $b(t)$ enters only once in the Dyson construction
- Derivative dependence: first-order only (and only in gate count) vs. arbitrary-order

The improvement over Fang-Lin-Tong is the $T$ scaling: $O(T)$ vs. $O(T^2)$, though that paper handles non-smooth dynamics and has different strengths.

## Limits / caveats

1. **Dissipative dynamics only.** The logarithmic-norm condition ($\mu(A(t)) \le 0$) is necessary for the condition number to be well-bounded. If the dynamics is unstable ($\mu > 0$), the solution can grow exponentially and the algorithm cost grows with it.

2. **Output is a quantum state.** The usual caveat: you get $|x(T)\rangle$ encoded in amplitudes. Extracting full classical information requires $\Omega(N)$ measurements. The speedup is real when you want global properties (expectation values, sampling) or the ODE arose from a quantum problem to begin with.

3. **The $R$ factor is messy.** In principle $R$ depends on the norms of the particular-solution vectors $v_m$ and the decay ratio $x_{\max}/\|x(T)\|$. For typical problems where the solution doesn't decay too much and $b(t)$ doesn't oscillate to produce cancellations, $R = O(1)$. But the full expression (Eq. 87 in the paper) is a reminder that pathological ODEs exist.

4. **No treatment of nonlinear or non-autonomous higher-order structure.** The paper works strictly with first-order linear systems. Higher-order ODEs can be reduced to first-order form, but nonlinear dynamics is out of scope.

5. **Gate complexity still depends on derivatives.** The oracle call count is derivative-free, but the gate count depends on $\log(D)$ where $D$ involves first derivatives of $A(t)$ and $b(t)$. For rapidly varying coefficients, this matters.

## My assessment

This is a clean, well-executed paper that fills an obvious gap. The prior work on time-dependent ODEs (Childs-Liu spectral method) had unnecessarily large log factors and needed smoothness assumptions. This paper removes both issues by switching from spectral methods to Dyson series, which is a natural choice given that the Dyson machinery was already available from the Hamiltonian simulation papers.

The time-independent result (Theorem 4.2) is almost a corollary, but it's worth having separately — it's simpler than the encoding in [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] and gives a cleaner complexity expression.

The paper's limitations are intrinsic to the problem class rather than artifacts of the approach: dissipative requirement, quantum state output, amplification cost for decaying solutions. Whether the remaining log factors are optimal is an interesting open question.

The acknowledgments thank Maria Kieferova for helpful discussions — not surprising given that the Dyson series toolkit comes directly from [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|her 2018 paper]].

## Reusable ideas

1. [[Dyson Block-Encoding for Time-Dependent Linear Systems]] — Using the Dyson series to block-encode the propagator $V_m$ within a [[History-State Linear System Encoding for ODE Trajectories|history-state linear system]], rather than encoding Taylor series terms as extra rows.

2. [[Logarithmic Norm Stability for ODE Condition Numbers]] — Bounding the condition number of the block-bidiagonal ODE linear system via the logarithmic norm $\mu(A(t))$, avoiding diagonalisability assumptions.

3. [[Decoupled Derivative Dependence in Dyson ODE Algorithms]] — Architectural trick: derivatives of the coefficients affect only the time-register discretization (gate count), not the number of oracle calls to block-encodings of $A(t)$ and $b(t)$.

## References within this paper

- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum ODE algorithm via [[History-State Linear System Encoding for ODE Trajectories|history-state encoding]] + HHL
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — Taylor-series linear system encoding for time-independent ODEs; the direct predecessor
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians; provides the block-encoding machinery
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]] — interaction-picture Dyson simulation; provides the alternative sort-vs-gap analysis
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2022)]] — optimal QLSA used as subroutine
- Childs and Liu, *Quantum spectral methods for differential equations*, Commun. Math. Phys. 375, 1427–1457 (2020) — spectral method for time-dependent ODEs; this paper's main comparison point
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — time-marching alternative with $O(T^2)$ scaling
- Krovi, *Improved quantum algorithms for linear and nonlinear differential equations*, Quantum 7, 913 (2023) — logarithmic-norm stability condition for time-independent case
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — HHL
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — preparation over non-power-of-2 basis states
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry et al. (2021)]] — inequality-testing approach for state preparation

## Cross-links

### Paper notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]

### Trick cards
- [[Dyson Block-Encoding for Time-Dependent Linear Systems]]
- [[Logarithmic Norm Stability for ODE Condition Numbers]]
- [[Decoupled Derivative Dependence in Dyson ODE Algorithms]]
- [[History-State Linear System Encoding for ODE Trajectories]]
- [[History-State Padding for Final-Time Readout]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Block-Encoding Composition Algebra]]
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — uses the truncated Taylor series ODE solver from this paper as the time-evolution subroutine for Carleman-linearised nonlinear ODEs
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — companion 2023 paper from overlapping authors; validates the practical efficiency of the optimal QLSP solver used in the ODE pipeline this paper establishes
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — extends adiabatic simulation with $\varepsilon$-independent time steps; advances the simulation-ODE pipeline further downstream
