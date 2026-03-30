> **Source:** Hari Krovi, *Improved quantum algorithms for linear and nonlinear differential equations*, Quantum 7:913, 2023; arXiv:2202.01054
> **Links:** [arXiv](https://arxiv.org/abs/2202.01054) · [Quantum](https://doi.org/10.22331/q-2023-02-02-913)
> **Tags:** #nonlinear-ODE #linear-ODE #Carleman-linearisation #quantum-algorithm #differential-equations #QLSA #block-encoding #log-norm #matrix-exponential

---

## The computational problem

Two problems, really:

**Linear ODE:** Solve $\dot{x} = Ax + b$, $x(0) = x_0$, where $A \in \mathbb{C}^{d \times d}$ is sparse and stable (all eigenvalues have negative real part). Produce a quantum state proportional to $x(T)$.

**Nonlinear ODE:** Solve $\dot{u} = F_2 u^{\otimes 2} + F_1 u + F_0$, $u(0) = u_{\mathrm{in}}$, same setting as [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu et al. (2021)]], but with the dissipativity condition relaxed from $\operatorname{Re}(\lambda_1) < 0$ (eigenvalue-based) to $\mu(F_1) < 0$ (log-norm-based), where $\mu(A) = \max\{\lambda : \lambda \in \sigma((A+A^\dagger)/2)\}$.

## What the paper does

Improves on both the linear ODE algorithm of [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] and the nonlinear ODE algorithm of [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu et al. (2021)]] in two ways:

1. **Characterises complexity via $\|e^{At}\|$ rather than eigenvalues of $A$.** Introduces the quantity $C(A) := \sup_{t \ge 0} \|e^{At}\|$ and shows the algorithm's complexity depends on $C(A)$, not on whether $A$ is diagonalisable or on $\kappa_V$ (condition number of the diagonalising matrix). This extends to non-diagonalisable, non-normal, and even singular matrices.

2. **Exponentially improves error dependence for nonlinear ODEs** from $\sim 1/\varepsilon$ (Liu et al.'s forward Euler) to $\mathrm{polylog}(1/\varepsilon)$ by using truncated Taylor series instead of forward Euler for the time discretisation.

For the nonlinear case, the log-norm condition $\mu(F_1) < 0$ is strictly weaker than requiring $F_1$ to be diagonalisable with all eigenvalues having negative real parts. The log-norm is the "symmetric part" of $F_1$, so it handles non-normal matrices naturally.

**My assessment:** The main conceptual contribution is the $C(A)$ characterisation — realising that $\|e^{At}\|$ is what actually matters for the ODE solver's complexity, not the spectral properties of $A$ alone. For normal matrices $C(A) = 1$ when $\mu \le 0$; for non-normal matrices $C(A)$ can be much larger than 1 even with $\alpha(A) < 0$ (transient growth), but it's still finite and computable. For the Carleman application, $C(A) \le N$ (the truncation order), which is clean.

The nonlinear improvement is somewhat incremental — it replaces forward Euler with truncated Taylor series, which is a natural step. The more lasting contribution is the $C(A)$/log-norm framework.

## The algorithm / construction

### Linear ODE algorithm

The linear system construction differs from [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry et al. (2017)]] and [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu et al. (2021)]]. Given $\dot{x} = Ax + b$ on $[0,T]$:

1. **Time discretisation:** Divide $[0,T]$ into $m$ steps of size $h$. Instead of forward Euler, use a $k$th-order truncated Taylor series:

$$W_k = \sum_{\ell=0}^{k} \frac{(Ah)^\ell}{\ell!} \approx e^{Ah}.$$

2. **Linear system:** The time-stepping $y_{i+1} = W_k y_i + \tilde{b}$ gives a block-bidiagonal system:

$$L\mathbf{y} = \mathbf{c},$$

where $L = \mathrm{diag}(I) - \mathrm{subdiag}(W_k)$ and $\mathbf{c}$ encodes the initial condition and forcing. The padding trick from [[History-State Padding for Final-Time Readout|Berry (2014)]] boosts the measurement probability.

3. **Block-encoding implementation:** $W_k$ is block-encoded using [[Block-Encoding Composition Algebra|block-encoding arithmetic]] — products and sums of block-encoded $A$ matrices. The [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] (specifically the optimal $O(\kappa \log(1/\varepsilon))$ version from Costa et al.) is applied to the resulting linear system.

**Key difference from Berry et al. (2017):** The condition number analysis uses $C(A)$ instead of $\kappa_V$. For the condition number bound: if $\|W_k\| \le 1$ (ensured by choosing $h \le 1/\|A\|$ and $k$ large enough), then $\kappa_L \le 3(2m+1)$ — same linear-in-$m$ scaling as forward Euler, but now with a much larger step size $h = 1/\|A\|$ since the higher-order method is more stable.

### Application to nonlinear ODEs

After [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] of $\dot{u} = F_2 u^{\otimes 2} + F_1 u + F_0$, the resulting linear system $\dot{x} = Ax + b$ has the Carleman block-tridiagonal matrix $A$.

**The key observation (Lemma 16):** If $\mu(F_1) < 0$ and $|{\mu(F_1)}| > \|F_0\| + \|F_2\|$ (i.e., $R < 1$ with $R$ now defined using log-norm), then $C(A) \le N$. The proof uses a diagonal preconditioning: $D = \mathrm{diag}(I/j)$ for $j = 1, \ldots, N$, and shows $\mu(DAD^{-1} + (DAD^{-1})^\dagger)/2) < 0$ via a block decomposition argument, which gives $\|e^{At}\| \le \kappa(D) = N$.

The dissipativity ratio becomes:

$$R = \frac{1}{|\mu(F_1)|}\left(\|F_2\|\|u_{\mathrm{in}}\| + \frac{\|F_0\|}{\|u_{\mathrm{in}}\|}\right).$$

Using log-norm instead of spectral abscissa is strictly more general: $\mu(A) \ge \alpha(A)$ always, so $\mu < 0$ implies $\alpha < 0$, but the converse can fail badly for non-normal matrices.

## Key results

| Result | Statement |
|---|---|
| Linear ODE query complexity (Theorem 7) | $O\!\left(g \cdot T\|A\| \cdot C(A) \cdot \mathrm{poly}\!\left(s, \log d, \log(1 + T\|b\|/\|x_T\|), \log(1/\varepsilon), \log(T\|A\|C(A))\right)\right)$ |
| Nonlinear ODE query complexity (Theorem 8) | $O\!\left(g_u \cdot T\|A\| \cdot \mathrm{poly}\!\left(N, s, \log(1 + \|F_0\|/\|u(T)\|), \log(1/\varepsilon), \log(T\|A\|)\right)\right)$ |
| $C(A)$ for Carleman system (Lemma 16) | $C(A) \le N$ when $\mu(F_1) < 0$ and $R < 1$ |
| Carleman truncation order | $N = O\!\left(\frac{\log(T\|F_2\|/\varepsilon\|u(T)\|)}{\log(1/\|u_{\mathrm{in}}\|)}\right)$ |
| Carleman error | $\|\eta_1(T)\| \le N^2 \|F_2\| T \|u(0)\|^{N+1}$ |
| Condition number of linear system (Theorem 4) | $\kappa_L \le 3(2m+1) \cdot C(A)$ where $m = \lceil T\|A\| \rceil$ |
| Solution error (Theorem 3) | $\|x_T - y_m\| \le \delta\|x_T\|$ for $k = O(\log \Omega / \log\log \Omega)$ |
| Measurement probability | $\Omega(1/(N g_u^2))$ where $g_u = \|u_{\mathrm{in}}\|/\|u(T)\|$ |

## Comparison with prior work

| | [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes\|Berry et al. (2017)]] | [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes\|Liu et al. (2021)]] | **This paper** |
|---|---|---|---|
| Matrix class | Diagonalisable, $\kappa_V$-dependent | Diagonalisable, $\operatorname{Re}(\lambda) < 0$ | General: $C(A) < \infty$ suffices |
| Error dependence (linear) | $\mathrm{polylog}(1/\varepsilon)$ | — | $\mathrm{polylog}(1/\varepsilon)$ |
| Error dependence (nonlinear) | — | $\sim 1/\varepsilon$ | $\mathrm{polylog}(1/\varepsilon)$ |
| Time dependence (nonlinear) | — | $T^2$ | $T \cdot \mathrm{poly}(\log T)$ |
| Stability condition | Diag. + real neg. eigenvalues | Diag. + $\operatorname{Re}(\lambda_1) < 0$ | $\mu(F_1) < 0$ (log-norm) |
| Non-normal matrices | Exponential penalty via $\kappa_V$ | Not handled | Efficient if $C(A)$ bounded |
| Measurement penalty | — | $O(\|u_{\mathrm{in}}\|^{2N})$ | $O(\|u_{\mathrm{in}}\|^{2N})$ (not fixed) |

A notable gap: this paper does *not* introduce the [[Carleman Vector Rescaling for Measurement Probability Boost|Carleman vector rescaling]] that eliminates the $\|u_{\mathrm{in}}\|^{2N}$ factor. The measurement probability is still $\Omega(1/(Ng_u^2))$, and for PDEs where $\|u_{\mathrm{in}}\|_2 \propto \sqrt{n}$, the cost is $n^N$ after the $\sqrt{N}$ from amplitude amplification. The rescaling fix came later in [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes|Costa et al. (2023)]].

## Limits / caveats

- **$R < 1$ still required.** The log-norm version is more general than the eigenvalue version, but the $R < 1$ condition itself is unchanged. Strongly nonlinear systems are still out of reach.

- **Measurement penalty not addressed.** The $\|u_{\mathrm{in}}\|^{2N}$ problem from Liu et al. (2021) persists. For PDE applications this is the binding constraint, and it wasn't fixed until Costa et al. (2023).

- **$C(A)$ can be hard to compute.** While $C(A) \le N$ for Carleman systems is clean, for general matrices $C(A) := \sup_{t \ge 0} \|e^{At}\|$ may not be easy to bound. The Kreiss matrix theorem gives $C(A) \le ed\mathcal{K}(A)$ where $\mathcal{K}(A)$ is the Kreiss constant, but this can be exponential in dimension.

- **Time-independent $F_0$ only.** Unlike Liu et al. (2021) which handles time-dependent $F_0(t)$, this paper restricts to time-independent forcing for simplicity. Not a fundamental limitation, but the analysis doesn't cover it.

- **No lower bound.** This paper doesn't prove any lower bounds, unlike Liu et al.'s $\Omega(2^T)$ for $R \ge \sqrt{2}$.

## Reusable ideas

1. [[Matrix Exponential Bound as ODE Solver Complexity Proxy]] — the insight that $C(A) = \sup_{t \ge 0} \|e^{At}\|$ is the right quantity for characterising quantum ODE solver complexity. Subsumes eigenvalue conditions, handles non-normal and non-diagonalisable matrices, and composes cleanly for block matrices (Carleman systems).

2. [[Log-Norm Diagonal Preconditioning for Carleman Systems]] — the trick of using $D = \mathrm{diag}(I/j)$ to show $C(A) \le \kappa(D) = N$ for the Carleman block matrix. The diagonal preconditioning converts a non-normal block matrix into one whose symmetrised version has negative log-norm.

## References within this paper

- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — the linear ODE algorithm this paper improves on
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu-Kolden-Krovi et al. (2021)]] — the nonlinear ODE algorithm this paper improves on (Krovi is a co-author on both)
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — the history-state + ramp approach
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa et al. (2021)]] — optimal QLSA ($O(\kappa \log(1/\varepsilon))$) used as subroutine
- Chakraborty-Gilyén-Jeffery (2019) — block-encoding arithmetic
- Gilyén-Su-Low-Wiebe (2019) — QSVT framework, block-encoding results
- Trefethen-Embree (2005) — pseudospectra, Kreiss matrix theorem
- Xue-Wu-Guo (2021) — homotopy perturbation method for nonlinear ODEs (alternative approach, worse $R$ condition)

## Cross-links

### Paper notes
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — the predecessor this paper directly improves
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — fixes the measurement probability problem this paper leaves open
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — the linear ODE algorithm improved here
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — foundational approach
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — QLSA subroutine
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — preconditioning ideas for linear systems
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]] — end-to-end PDE complexity analysis
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — alternative linear ODE approach

### Trick cards
- [[Matrix Exponential Bound as ODE Solver Complexity Proxy]] — the $C(A)$ characterisation *(new)*
- [[Log-Norm Diagonal Preconditioning for Carleman Systems]] — the diagonal preconditioning trick *(new)*
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the core nonlinear technique
- [[Forward Euler to Linear System Reduction]] — the method this paper improves on
- [[Block-Encoding Composition Algebra]] — used for constructing the linear system
- [[History-State Padding for Final-Time Readout]] — the ramp for measurement probability
