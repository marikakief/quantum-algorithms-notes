> **Source:** D. W. Berry, *High-order quantum algorithm for solving linear differential equations*, J. Phys. A: Math. Theor. **47**, 105301 (2014); arXiv:1010.2745
> **Links:** [arXiv](https://arxiv.org/abs/1010.2745) · [JPhysA](https://doi.org/10.1088/1751-8113/47/10/105301)
> **Tags:** #quantum-algorithm #differential-equations #linear-systems #HHL #history-state #multistep-methods

---

## The problem

Solve a sparse system of first-order linear ODEs:
$$\dot{x}(t) = A(t)x(t) + b(t)$$
where $A$ is an $N_x \times N_x$ sparse matrix, $x$ and $b$ are $N_x$-component vectors. Goal: encode $x(t_0 + \Delta t)$ in a quantum state in time $O(\text{poly}\log N_x)$.

Quantum mechanics is a special case ($b = 0$, $A = iH$ with $H$ Hermitian). General linear ODEs cover classical physics: heat equations, Maxwell's equations, Stokes flow, and any PDE discretised on a mesh.

## What the paper does

First algorithm for general (non-Schrödinger) linear ODEs on a quantum computer with polynomial scaling in the evolution time. The key idea: encode the ODE trajectory at all times as a single quantum state using a [[History-State Encoding with Unary Clock|Feynman clock / history state]], then solve the resulting linear system with [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]].

Main result: complexity $\tilde{O}(\text{poly}\log(N_x) \cdot s^{9/2} \cdot (\|A\|\Delta t)^{2+2/p} \cdot \kappa_V^5 / \varepsilon^2)$ oracle calls, where $p$ is the order of the multistep method, $s$ is the sparsity, $\kappa_V$ is the condition number of the eigenvector matrix, and $\varepsilon$ is the error.

For high-order methods ($p \to \infty$), the dominant scaling approaches $\tilde{O}((\|A\|\Delta t)^2)$.

---

## The algorithm

### Step 1: Discretise the ODE as a linear system

Discretise time into $N_t$ steps of size $h$. The trick: encode the solution at *all* time steps simultaneously as a single quantum state (the [[History-State Encoding with Unary Clock|history state]]):

$$|\psi\rangle = \sum_{j=0}^{N_t} |t_j\rangle |x_j\rangle$$

The discretised ODE becomes a block-bidiagonal (or block-banded for multistep methods) linear system:

$$\mathcal{A}\vec{x} = \vec{b}$$

For the Euler method, $\mathcal{A}$ has block structure:

$$\begin{pmatrix} I & & & \\ -(I+Ah) & I & & \\ & -(I+Ah) & I & \\ & & -I & I \end{pmatrix} \begin{pmatrix} x_0 \\ x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} x_{\text{in}} \\ bh \\ bh \\ 0 \end{pmatrix}$$

The first row sets the initial condition. The middle rows encode the ODE stepping. The last rows enforce $x_{j+1} = x_j$ for times beyond $t_0 + \Delta t$, so that measuring the clock register in the second half always yields the final answer. This boosts the success probability from $1/(N_t+1)$ to $\sim 1/2$.

### Step 2: High-order multistep methods

The Euler method gives $\tilde{O}(\Delta t^4)$ scaling — poor because the condition number $\kappa \sim N_t \sim \Delta t^2/\varepsilon$ and HHL costs $\kappa^2$.

Fix: use $k$-step linear multistep methods of order $p = k$. These replace the Euler recurrence with:

$$\sum_{\ell=0}^{k} \alpha_\ell x_{j+\ell} = h \sum_{\ell=0}^{k} \beta_\ell [A(t_{j+\ell}) x_{j+\ell} + b(t_{j+\ell})]$$

For order $p$, the number of time steps needed is $N_t = O((\|A\|\Delta t)^{1+1/p} / \varepsilon^{1/p})$ — close to linear in $\Delta t$ for large $p$.

### Step 3: Condition number analysis

The condition number of $\mathcal{A}$ scales as $\kappa = O(N_t \kappa_V)$. The key insight: each excitation in $\vec{b}$ can cause at most a bounded displacement for all future times (because $A$-stability prevents growth), so $\|\mathcal{A}^{-1}\| = O(N_t \kappa_V)$. Since $\|\mathcal{A}\| = O(1)$ when $h = O(1/\|A\|)$, we get $\kappa = O(N_t \kappa_V)$.

### Step 4: Solve with HHL

Apply [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] to the system $\mathcal{A}\vec{x} = \vec{b}$. HHL costs $\tilde{O}(\log(N) s^4 \kappa^2 / \varepsilon_L)$. The state preparation for $\vec{b}$ costs $O(\sqrt{s} + \log N_t)$. Combining everything gives the main result.

---

## Key results

| Result | Statement |
|---|---|
| Euler method complexity | $\tilde{O}(\text{poly}\log(N_x) \cdot s^{9/2} \cdot (\|A\|\Delta t)^4 \cdot \kappa_V^5 / \varepsilon^2)$ |
| High-order ($p$th order) complexity | $\tilde{O}(\text{poly}\log(N_x) \cdot s^{9/2} \cdot (\|A\|\Delta t)^{2+2/p} \cdot \kappa_V^5 / \varepsilon^2)$ |
| Asymptotic ($p \to \infty$) | $\tilde{O}((\|A\|\Delta t)^2)$ in evolution time |
| Condition number | $\kappa(\mathcal{A}) = O(N_t \kappa_V)$ |
| Lower bound | $\Omega(\|A\|\Delta t)$ from no-fast-forwarding |

---

## The history state trick for ODEs

This is the paper that transplants Kitaev's clock construction from complexity theory into algorithms. The idea had been used in:
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — original construction for QMA-completeness
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — adiabatic-to-circuit equivalence

Berry's insight: the same encoding works for *classical* ODE trajectories. Instead of encoding the intermediate states of a quantum circuit, encode the intermediate values of a discretised ODE. The time register plays the role of the clock, and the linear system enforces correct propagation between time steps — exactly as the propagation Hamiltonian enforces correct gate application in Kitaev's construction.

This idea spawned an entire subfield. Follow-ups:
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — exponentially improved precision ($\text{polylog}(1/\varepsilon)$ instead of $\text{poly}(1/\varepsilon)$) by encoding the Taylor series of the propagator as an LCU within the history state
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes|Babbush-Berry-Kothari-Somma-Wiebe (2023)]] — exponential speedup for coupled oscillators by mapping Newton's equations to block-Hamiltonian evolution
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|Childs-Liu-Ostrander (2021)]] — extends to PDEs via spectral methods
- Costa, An, Sanders, Su, Babbush, Berry (2022) — optimal ODE algorithm with $O(\text{poly}\log(1/\varepsilon))$ and near-linear $\Delta t$ scaling

---

## Comparison with prior and subsequent work

| | Berry (2014) | Berry-Childs-Ostrander-Wang (2017) | Costa et al. (2022) |
|---|---|---|---|
| $\Delta t$ scaling | $\tilde{O}((\|A\|\Delta t)^{2+2/p})$ | $\tilde{O}((\|A\|\Delta t)^2)$ | $O(\|A\|\Delta t \cdot \text{polylog})$ |
| $\varepsilon$ scaling | $\text{poly}(1/\varepsilon)$ | $\text{polylog}(1/\varepsilon)$ | $\text{polylog}(1/\varepsilon)$ |
| Approach | History state + HHL | History state + LCU/Taylor | Hamiltonian simulation (direct) |
| Key innovation | History state for ODEs | Taylor-series propagator | Spectral methods + Hamiltonian embedding |

---

## Limits / caveats

- **Quadratic gap from optimal.** Scaling $\tilde{O}(\Delta t^2)$ vs. the $\Omega(\Delta t)$ lower bound. The quadratic penalty comes from HHL's $\kappa^2$ dependence. Variable time amplitude amplification was investigated (Section VII) but found not to help due to the recursive overhead in the Estimate procedure.
- **$A$ must be diagonalisable** with eigenvalues in a left-half-plane wedge ($|arg(-\lambda_i)| \leq \alpha$). This is needed for $A(\alpha)$-stability of the multistep method. Systems with eigenvalues near the imaginary axis need care.
- **Condition number $\kappa_V$** of the eigenvector matrix enters polynomially. For normal matrices $\kappa_V = 1$; for highly non-normal matrices this can blow up.
- **Output is encoded in amplitudes.** You get a quantum state proportional to $x(\Delta t)$, not the explicit vector. Extracting individual components costs $O(N_x)$. Only global features (expectation values, norms, inner products) are efficiently accessible.
- **Constant coefficients only** (rigorous result). Time-dependent coefficients are discussed informally but without rigorous error bounds.
- **Sparsity assumption.** $A$, $b$, and $x_{\text{in}}$ must be $s$-sparse with efficient oracle access.

---

## Reusable ideas

1. [[History-State Linear System Encoding for ODE Trajectories]] — encode the discretised ODE trajectory at all time steps as a single quantum state, convert to a block-banded linear system, solve with a quantum linear system solver. The time register is a Feynman clock adapted from Kitaev's QMA construction.

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the HHL algorithm used as the main subroutine
- Feynman (1982, 1985) — original clock construction ("Feynman's computer")
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — formalised history state for QMA-completeness
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — quantum simulation of Hamiltonian dynamics (the $A = iH$ special case)
- Berry-Ahokas-Cleve-Sanders (2007) — efficient sparse Hamiltonian simulation
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry-Childs (2011)]] — black-box Hamiltonian simulation
- Leyton-Osborne (2008) — earlier attempt at nonlinear differential equations (exponential in time steps)
- Ambainis (2010, 2012) — variable time amplitude amplification (explored but found insufficient)

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL, the linear system solver
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — direct follow-up, exponentially improved precision
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]] — applies the ODE-to-Hamiltonian idea to coupled oscillators
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]] — PDE extension
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — the history state construction Berry adapts
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] — Kitaev's clock, the original source

### Trick cards
- [[History-State Linear System Encoding for ODE Trajectories]]
- [[History-State Encoding with Unary Clock]] — the underlying Kitaev clock construction
- [[History-State Padding for Final-Time Readout]] — the padding trick to boost success probability
