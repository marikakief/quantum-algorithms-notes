> **Source:** Jin-Peng Liu, Herman Øie Kolden, Hari K. Krovi, Nuno F. Loureiro, Konstantina Trivisa, Andrew M. Childs, *Efficient quantum algorithm for dissipative nonlinear differential equations*, Proceedings of the National Academy of Sciences 118(35):e2026805118, 2021; arXiv:2011.03185
> **Links:** [arXiv](https://arxiv.org/abs/2011.03185) · [PNAS](https://doi.org/10.1073/pnas.2026805118)
> **Tags:** #nonlinear-ODE #Carleman-linearisation #quantum-algorithm #differential-equations #QLSA #lower-bound #dissipative

---

## The computational problem

Solve an $n$-dimensional quadratic ODE

$$\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}t} = F_2 \mathbf{u}^{\otimes 2} + F_1 \mathbf{u} + F_0(t), \qquad \mathbf{u}(0) = \mathbf{u}_{\mathrm{in}},$$

where $F_2 \in \mathbb{R}^{n \times n^2}$, $F_1 \in \mathbb{R}^{n \times n}$, $F_0(t) \in \mathbb{R}^n$, and $F_1$ has all eigenvalues with strictly negative real part. The dissipativity ratio is

$$R := \frac{1}{|\operatorname{Re}(\lambda_1)|}\left(\|\mathbf{u}_{\mathrm{in}}\|\|F_2\| + \frac{\|F_0\|}{\|\mathbf{u}_{\mathrm{in}}\|}\right),$$

where $\lambda_1$ is the eigenvalue of $F_1$ closest to the imaginary axis. Assume $R < 1$. The goal: produce a quantum state proportional to $\mathbf{u}(T)$.

Higher-degree polynomial nonlinearities reduce to the quadratic case by introducing auxiliary variables, so restricting to quadratic is without loss of generality.

## What the paper does

This is the paper that made quantum nonlinear DE solving a real research direction. It introduces [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] to quantum computing: embed the quadratic ODE into an infinite-dimensional linear system by lifting to tensor powers of $\mathbf{u}$, truncate at order $N$, discretise with forward Euler, and solve with a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|quantum linear system algorithm]]. Under $R < 1$, the complexity is $T^2 q \cdot \mathrm{poly}(\log T, \log n, \log 1/\varepsilon)/\varepsilon$, exponentially better in $T$ than the previous best of $O(1/\varepsilon^T)$ from Leyton-Osborne.

The paper also proves a lower bound: for $R \ge \sqrt{2}$, the problem is intractable (exponential in $T$) for any quantum algorithm — not just Carleman-based ones. The gap $1 \le R < \sqrt{2}$ remains open.

## The algorithm / construction

### Step 1: Carleman linearisation

Define $\hat{y}_j = \mathbf{u}^{\otimes j}$ for $j = 1, \ldots, N$ and collect into a vector $\hat{\mathbf{y}} = [\hat{y}_1; \hat{y}_2; \ldots; \hat{y}_N]$. The derivatives satisfy:

$$\frac{\mathrm{d}\hat{y}_j}{\mathrm{d}t} = A_j^{(1)}\hat{y}_j + A_{j+1}^{(2)}\hat{y}_{j+1} + A_{j-1}^{(0)}\hat{y}_{j-1},$$

where $A_j^{(1)} = \sum_{\ell=1}^{j} I^{\otimes(\ell-1)} \otimes F_1 \otimes I^{\otimes(j-\ell)}$, and similarly for $A_{j+1}^{(2)}$ (with $F_2$) and $A_{j-1}^{(0)}$ (with $F_0(t)$). This is a tridiagonal block structure. The inhomogeneous Carleman matrix is $(3Ns)$-sparse with total dimension $\Delta = \sum_{j=1}^{N} n^j = O(n^N)$, requiring $O(N \log n)$ qubits.

Truncation at order $N$ drops the coupling from $\hat{y}_N$ to $\hat{y}_{N+1}$, introducing the Carleman truncation error.

### Step 2: Forward Euler discretisation

Divide $[0, T]$ into $m = T/h$ time steps and apply forward Euler:

$$\mathbf{y}_{k+1} = [I + A(kh)h]\mathbf{y}_k + \mathbf{b}(kh).$$

This produces an $(m+p+1)\Delta \times (m+p+1)\Delta$ linear system $L|\mathbf{Y}\rangle = |\mathbf{B}\rangle$, where $L$ is lower-triangular (block bidiagonal) and $p$ extra time steps copy the final solution to improve the [[History-State Padding for Final-Time Readout|measurement success probability]].

### Step 3: Rescaling

Before everything, the ODE is rescaled $\mathbf{u} \to \gamma\mathbf{u}$ so that $\|\mathbf{u}_{\mathrm{in}}\| < 1$ and $\|F_2\| + \|F_0\| < |\operatorname{Re}(\lambda_1)|$. The parameter $R$ is invariant under rescaling. The specific choice $\gamma = 1/\sqrt{\|\mathbf{u}_{\mathrm{in}}\| r_+}$ (where $r_+$ is the larger root of $\|F_2\|x^2 + \operatorname{Re}(\lambda_1)x + \|F_0\| = 0$) ensures both conditions hold.

This rescaling is much simpler than the [[Carleman Vector Rescaling for Measurement Probability Boost|Carleman vector rescaling]] introduced in Costa-Schleich-Morales-Berry (2023). It ensures $\|\mathbf{u}_{\mathrm{in}}\| < 1$ but does *not* eliminate the exponential-in-$N$ measurement probability penalty — that's a later innovation.

### Step 4: QLSA + measurement

Apply the [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] (specifically the Childs-Kothari-Somma 2017 version) to solve $L|\mathbf{Y}\rangle = |\mathbf{B}\rangle$, then postselect on the final time register and the $j=1$ Carleman component to extract $\mathbf{u}(T)/\|\mathbf{u}(T)\|$.

### Step 5: Amplitude amplification

The measurement probability is $\Omega(1/Nq^2)$ where $q = \|\mathbf{u}_{\mathrm{in}}\|/\|\mathbf{u}(T)\|$. Apply [[Standard Amplitude Amplification|amplitude amplification]] to boost to constant probability, costing $O(\sqrt{N}q)$ repetitions.

## Key results

| Result | Statement |
|---|---|
| Main complexity (Theorem 1) | $\displaystyle \frac{sT^2 q[(\|F_2\|+\|F_1\|+\|F_0\|)^2 + \|F_0'\|]}{(1-\|\mathbf{u}_{\mathrm{in}}\|)^2(\|F_2\|+\|F_0\|)g\varepsilon} \cdot \mathrm{poly}\!\left(\frac{\log(\cdots)}{\log(1/\|\mathbf{u}_{\mathrm{in}}\|)}\right)$ |
| Simplified (real eigenvalues) | $\displaystyle \frac{sT^2 q[(\|F_2\|+\|F_1\|+\|F_0\|)^2 + \|F_0'\|]}{g\varepsilon} \cdot \mathrm{poly}\!\left(\frac{\log(\cdots)}{\log(1/\|\mathbf{u}_{\mathrm{in}}\|)}\right)$ |
| Carleman truncation order | $N = O\!\left(\frac{\log(T\|F_2\|/g\varepsilon)}{\log(1/\|\mathbf{u}_{\mathrm{in}}\|)}\right)$ |
| Carleman error (Lemma 2) | $\|\eta_j(t)\| \le tN\|F_2\|\|\mathbf{u}_{\mathrm{in}}\|^{N+1}$ |
| Carleman error, homogeneous (Corollary 1) | $\|\eta_1(t)\| \le \|\mathbf{u}_{\mathrm{in}}\| R^N |1 - e^{\operatorname{Re}(\lambda_1)t}|^N$ |
| Euler global error (Lemma 3) | $\|\hat{y}_1(T) - y_1^m\| \le 3N^{2.5}Th[(\|F_2\|+\|F_1\|+\|F_0\|)^2 + \|F_0'\|]$ |
| Condition number (Lemma 4) | $\kappa \le 3(m+p+1)$ |
| Lower bound (Theorem 2) | For $R \ge \sqrt{2}$, any quantum algorithm has worst-case complexity $2^{\Omega(T)}$ |

## Comparison with prior work

| | Leyton-Osborne (2008) | **This paper** ($R < 1$) | [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes\|Costa et al. (2023)]] |
|---|---|---|---|
| Time dependence | $O(1/\varepsilon^T)$ (exponential) | $T^2 \cdot \mathrm{poly}(\log T)$ | $T \cdot \mathrm{poly}(\log T)$ |
| Error dependence | embedded in exp | $\sim 1/\varepsilon$ | $\log(1/\varepsilon)$ |
| Measurement penalty | — | $O(\|\mathbf{u}_{\mathrm{in}}\|^{2N})$ | $O(1/\sqrt{1 - R^{2/(M-1)}})$ |
| Nonlinearity | polynomial | quadratic ($M=2$) | general $M$ |
| Method | copy-based | Carleman + forward Euler + QLSA | Carleman + rescaling + Taylor ODE + QLSA |

The $T^2$ dependence comes from the forward Euler discretisation requiring $m = T/h$ time steps with $h = O(1/(NT))$-scale step size. Later work by [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes|Costa et al. (2023)]] replaces forward Euler with a [[Taylor-Series-to-Linear-System Encoding|Taylor series ODE solver]], bringing this down to near-linear in $T$.

The $1/\varepsilon$ error dependence is intrinsic to forward Euler (first-order method). The Childs-Kothari-Somma QLSA contributes only $\mathrm{polylog}(1/\varepsilon)$, so the bottleneck is the classical discretisation. Again, the Taylor series approach of Costa et al. fixes this.

The $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ measurement penalty is the biggest problem for applications. For PDEs with $n$ grid points, $\|\mathbf{u}_{\mathrm{in}}\|_2 \propto \sqrt{n}$, making the cost $n^N$ — superlinear and fatal. The [[Carleman Vector Rescaling for Measurement Probability Boost|Carleman vector rescaling]] of Costa et al. (2023) eliminates this.

## The lower bound

This is independently interesting. For $R \ge \sqrt{2}$, the paper constructs a 2D quadratic ODE where two initial states with overlap $1 - \varepsilon$ evolve to states with constant overlap in time $T = O(\log(1/\varepsilon))$. Since distinguishing states with overlap $1 - \varepsilon$ requires $\Omega(1/\varepsilon)$ copies (Helstrom bound), simulating this ODE for time $T$ requires $\Omega(1/\varepsilon) = \Omega(2^T)$ queries.

The construction uses $\mathrm{d}u_j/\mathrm{d}t = -u_j + Ru_j^2$ whose solution is $u_j(t) = 1/(R - e^t(R - 1/u_j(0)))$. For $u_j(0) > 1/R$, the solution blows up in finite time — the nonlinearity amplifies small differences exponentially, enabling state discrimination. This is a clean lower bound that rules out *any* quantum approach (not just Carleman) for large $R$.

The gap at $1 \le R < \sqrt{2}$ is still open as of 2025.

## Limits / caveats

- **$R < 1$ is restrictive.** This means the dissipative linear term must dominate both the nonlinearity and the forcing. Navier-Stokes at moderate-to-high Reynolds number has $R \gg 1$. The paper shows $R \approx 44$ for a viscous Burgers equation — way outside the proven regime — though numerically the Carleman method still converges there. Why it works for $R > 1$ in that case is an open question.

- **Forward Euler gives $T^2/\varepsilon$ scaling.** Both the quadratic time dependence and linear precision dependence are artefacts of using first-order discretisation. This was the main technical limitation addressed by subsequent work: [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes|Costa et al. (2023)]] and [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi (2023)]].

- **Measurement probability penalty.** Without rescaling, the probability of extracting the solution component $y_1$ is $O(\|\mathbf{u}_{\mathrm{in}}\|^{-2N})$, which is exponentially small in $N$ when $\|\mathbf{u}_{\mathrm{in}}\| > 1$. For PDEs this is fatal. Fixed by [[Carleman Vector Rescaling for Measurement Probability Boost|Carleman vector rescaling]].

- **Homogeneous equations have exponential decay.** When $F_0 = 0$ and $R < 1$, the solution norm decays exponentially: $\|\mathbf{u}(t)\| \sim e^{-|\operatorname{Re}(\lambda_1)|t}$. The factor $q = \|\mathbf{u}_{\mathrm{in}}\|/\|\mathbf{u}(T)\|$ then grows exponentially, destroying efficiency. Inhomogeneous equations with nonzero $F_0$ can maintain the solution norm and avoid this.

- **Output is a quantum state.** Extracting full classical information costs $O(n)$. Only global properties (expectation values, norms, sampling) are efficiently accessible.

- **No qualitative nonlinear phenomena.** The truncated Carleman system is linear and cannot capture bifurcations, chaos, or blowup. The regime $R < 1$ may inherently preclude chaos (not proven, but plausible).

## Reusable ideas

1. [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the core embedding: lift $\mathbf{u}$ to $(\mathbf{u}, \mathbf{u}^{\otimes 2}, \ldots, \mathbf{u}^{\otimes N})$ to convert polynomial nonlinear ODEs to linear ones. The infinite-dimensional Hilbert space is natural for a quantum computer.

2. [[Forward Euler to Linear System Reduction]] — discretise a linear ODE $\dot{\hat{\mathbf{y}}} = A\hat{\mathbf{y}} + \mathbf{b}$ via forward Euler, then write the recurrence as a block-bidiagonal linear system solvable by QLSA. The condition number is $O(m)$ (number of time steps) — controlled because $\|I + Ah\| \le 1$ under a CFL-like condition.

3. [[Nonlinear ODE Hardness via State Discrimination]] — for $R \ge \sqrt{2}$, quadratic ODEs can amplify small state differences to constant differences in time $O(\log(1/\varepsilon))$, proving $\Omega(2^T)$ lower bounds via the Helstrom bound. A template for proving hardness of other nonlinear simulation problems.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the original QLSA
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum linear ODE algorithm
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — Taylor-series ODE solver with exponentially improved precision
- Childs-Kothari-Somma (2017) — QLSA used in this paper's implementation
- Leyton-Osborne (2008) — previous quantum nonlinear ODE algorithm with $O(1/\varepsilon^T)$ complexity
- Carleman (1932) — the original linearisation technique
- Forets-Pouly (2017, Ref. [30]) — prior convergence analysis of Carleman linearisation (homogeneous case only); this paper extends to inhomogeneous
- Abrams-Lloyd (1998) and Meyer-Wong (2013, Ref. [21]) — earlier work on computational power of nonlinear quantum mechanics, showing state discrimination enables solving hard problems

## Cross-links

### Paper notes
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — direct successor: replaces forward Euler with Taylor series, adds rescaling, generalises to degree-$M$ nonlinearities
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] — improves this paper via QSVT-based solver and preconditioning for stiff systems
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — QLSA subroutine
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — the history-state ODE encoding this paper adapts
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — Taylor-series linear ODE solver
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — later linear ODE solver whose techniques Costa et al. (2023) apply to the Carleman system
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — optimal QLSA used in later Carleman work
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]] — similarly honest about when quantum speedups survive end-to-end for PDEs
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — preconditioning ideas later applied to Carleman systems by Krovi

### Trick cards
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the core technique
- [[Forward Euler to Linear System Reduction]] — the time discretisation approach *(new)*
- [[Nonlinear ODE Hardness via State Discrimination]] — the lower bound technique *(new)*
- [[Carleman Vector Rescaling for Measurement Probability Boost]] — later fix for the measurement probability problem identified here
- [[Norm Tension Between PDE Stability and Quantum Rescaling]] — the max-norm vs 2-norm issue first implicit in this paper
- [[History-State Linear System Encoding for ODE Trajectories]] — encoding used for the linearised system
