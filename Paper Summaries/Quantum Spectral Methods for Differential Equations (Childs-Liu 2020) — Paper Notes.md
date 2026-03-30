> **Source:** Andrew M. Childs and Jin-Peng Liu, *Quantum spectral methods for differential equations*, Communications in Mathematical Physics 375:1427–1457, 2020; arXiv:1901.00961
> **Links:** [arXiv](https://arxiv.org/abs/1901.00961) · [CMP](https://doi.org/10.1007/s00220-019-03575-z)
> **Tags:** #linear-ODE #spectral-method #Chebyshev #quantum-algorithm #differential-equations #QLSA #boundary-value-problem #time-dependent

---

## The computational problem

Solve a $d$-dimensional system of linear ODEs with time-dependent coefficients:

$$\frac{\mathrm{d}x(t)}{\mathrm{d}t} = A(t)x(t) + f(t), \qquad x(0) = \gamma,$$

where $A(t) \in \mathbb{C}^{d \times d}$ is $s$-sparse for all $t$, $A(t)$ is diagonalisable with all eigenvalues having non-positive real parts, and $f(t)$ is the forcing term. Produce a quantum state proportional to $x(T)$.

Also handles boundary value problems: given $\alpha x(0) + \beta x(T) = \gamma$, produce a state proportional to $x(t^*)$ for any desired $t^*$.

## What the paper does

Introduces **spectral methods** (specifically, Chebyshev pseudospectral methods) to quantum ODE solving. Instead of discretising time into small steps (finite differences / forward Euler / Taylor series), approximate the entire solution globally as a truncated Chebyshev series:

$$x_i(t) = \sum_{k=0}^{n} c_{i,k} T_k(t),$$

where $T_k$ are Chebyshev polynomials. The coefficients $c_{i,k}$ are determined by a linear system encoding the ODE at Chebyshev-Gauss-Lobatto interpolation nodes.

The result: **$\mathrm{poly}(\log(1/\varepsilon))$ complexity for time-dependent linear ODEs** — the first algorithm to achieve this. Previous work by [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry et al. (2017)]] achieved $\mathrm{polylog}(1/\varepsilon)$ only for time-independent equations. [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] handled time-dependent equations but with $\mathrm{poly}(1/\varepsilon)$ scaling.

**My assessment:** This is a clean, elegant paper. The core insight — use a global (spectral) approximation instead of local (finite difference) methods — is natural from a numerical analysis perspective but hadn't been brought to quantum algorithms before. The exponential convergence of spectral methods for smooth solutions is the key: if the solution is $C^\infty$, the Chebyshev approximation error decreases faster than any polynomial of $1/n$, so $n = \mathrm{polylog}(1/\varepsilon)$ suffices. The construction is straightforward once you see the idea: the Chebyshev coefficients satisfy a linear system, which you hand to the [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]].

The BVP extension is a nice bonus — BVPs are naturally suited to spectral methods (they're global problems), and the quantum algorithm handles them with similar complexity.

This paper is an ancestor of the Carleman papers by the same authors: Childs and Liu later applied similar ideas to nonlinear ODEs via [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Carleman linearisation]].

## The algorithm / construction

### Step 1: Chebyshev pseudospectral discretisation

Map $[0, T]$ onto $[-1, 1]$. For stability, divide into $m = O(\|A\|T)$ subintervals, each mapped to $[-1,1]$ separately. On each subinterval, approximate the solution by a degree-$n$ Chebyshev series.

The Chebyshev-Gauss-Lobatto nodes are $t_l = \cos(l\pi/n)$ for $l = 0, \ldots, n$. At these nodes, enforce the ODE:

$$\sum_{k=0}^{n} c'_{i,k} T_k(t_l) = \sum_{j=0}^{d-1} A_{ij}(t_l) \sum_{k=0}^{n} c_{j,k} T_k(t_l) + f_i(t_l),$$

where the differentiation coefficients $c'_{i,k}$ relate to $c_{i,k}$ via the Chebyshev differentiation matrix $D_n$ (upper triangular, entries $[D_n]_{kj} = 2j/\sigma_k$ for $k + j$ odd, $j > k$).

### Step 2: Linear system construction

Stack the Chebyshev coefficients across all subintervals into a single linear system $L|X\rangle = |B\rangle$.

The matrix $L$ has block structure connecting adjacent subintervals (the solution at the end of one subinterval is the initial condition for the next). The key blocks are:
- $L_1$: encodes the ODE at interpolation nodes (Chebyshev differentiation + ODE matrix)
- $L_2$: encodes continuity between subintervals
- $L_3$: extracts the solution at the desired output time

Add $p$ padding blocks (as in [[History-State Padding for Final-Time Readout|Berry (2014)]]) to boost measurement probability.

### Step 3: QLSA + measurement

Apply the high-precision [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] (Childs-Kothari-Somma 2017 version) to $L|X\rangle = |B\rangle$. Measure the time register to obtain the final-time subinterval, then extract $x(T)$.

### Step 4: Amplitude amplification

Measurement success probability is $\Omega(n/(mq^2))$. Apply amplitude amplification $O(q/\sqrt{n})$ times.

### Spectral convergence

The critical point: for solutions in $C^{r+1}$, the spectral method error is $O(\|x^{(n+1)}\|/n^{r-2})$ (Lemma 1). For $C^\infty$ solutions (which time-dependent linear ODEs with smooth coefficients produce), the error is $O(g' \cdot (e/(2n))^n)$ (Lemma 2) — super-exponential in $n$. So $n = O(\log \Omega / \log\log \Omega)$ suffices for error $\delta$, where $\Omega$ involves $g'$ (a smoothness parameter).

This is where the exponential improvement over finite-difference methods comes from: finite differences give $O(h^k)$ error requiring $n = O(1/\varepsilon^{1/k})$ points, while spectral methods give $O((e/2n)^n)$ error requiring $n = O(\log(1/\varepsilon))$ points.

## Key results

| Result | Statement |
|---|---|
| IVP query complexity (Theorem 1) | $O\!\left(\kappa_V s \|A\| T q \cdot \mathrm{poly}\!\left(\log(\kappa_V s \|A\| g' T / \varepsilon g)\right)\right)$ |
| BVP query complexity (Theorem 2) | $O\!\left(\kappa_V s \|A\|^4 T^4 q \cdot \mathrm{poly}\!\left(\log(\kappa_V s \|A\| g' T / \varepsilon g)\right)\right)$ |
| Time-independent IVP (Corollary 1) | Same as Theorem 1 with $g' \to \|\gamma\| + 2\tau\|f\|$ |
| Spectral error ($C^\infty$ solution) | $\max_t \|x(t) - \hat{x}(t)\| \le m g' (e/(2n))^n$ |
| Spectral error ($C^{r+1}$ solution) | $\max_t \|x(t) - \hat{x}(t)\| \le C \|x^{(n+1)}\| / n^{r-2}$ |
| Condition number (Lemma 4) | $\kappa_L \le (\pi m + p + 1)(n+1)^{3.5}(2\kappa_V + e\|\gamma\|_\infty)$ |
| Chebyshev series order | $n = O\!\left(\frac{\log \Omega}{\log\log \Omega}\right)$ where $\Omega = g' e^m / \delta$ |
| Measurement probability (Lemma 5) | $\ge \frac{(p+1)(n+1)}{\pi m q^2 + (p+1)(n+1)}$ |

## Comparison with prior work

| | [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes\|Berry (2014)]] | [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes\|Berry et al. (2017)]] | **This paper** | [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes\|Berry-Costa (2022)]] |
|---|---|---|---|---|
| Error dependence | $\mathrm{poly}(1/\varepsilon)$ | $\mathrm{polylog}(1/\varepsilon)$ | $\mathrm{polylog}(1/\varepsilon)$ | $\mathrm{polylog}(1/\varepsilon)$ |
| Time-dependent $A(t)$ | Yes (in principle) | No | **Yes** | Yes |
| Time dependence | $\sim T^2$ | $\sim T$ | $\sim T \cdot \mathrm{polylog}(T)$ | $\sim T \cdot \mathrm{polylog}(T)$ |
| Method | Finite difference + QLSA | Taylor series + QLSA | Chebyshev spectral + QLSA | Dyson series + QLSA |
| Matrix class | Diagonalisable | Diagonalisable | Diagonalisable ($\kappa_V$-dependent) | General (via [[Matrix Exponential Bound as ODE Solver Complexity Proxy\|$C(A)$]]) |
| BVP support | No | No | **Yes** | No |
| Smoothness requirement | None beyond ODE | None beyond ODE | $C^\infty$ for polylog; $C^{r+1}$ for poly | Bounded derivatives |

The $\kappa_V$ dependence is a weakness compared to [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi (2023)]], which eliminates it via the $C(A)$ characterisation. The [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry-Costa (2022)]] Dyson series approach later achieved similar $\mathrm{polylog}(1/\varepsilon)$ for time-dependent equations without the spectral method machinery, and with better constant factors.

## Limits / caveats

- **Smoothness requirement.** The exponential convergence of Chebyshev approximation requires the solution to be $C^\infty$ (or at least $C^{r+1}$ for $O(1/n^{r-2})$ convergence). Solutions with discontinuities or low regularity degrade to algebraic convergence. In practice, linear ODEs with smooth $A(t)$ and $f(t)$ produce smooth solutions, so this is rarely a binding constraint.

- **$\kappa_V$ dependence.** The complexity depends on $\kappa_V = \max_t \kappa(V(t))$, the condition number of the diagonalising matrix. For non-normal $A(t)$, this can be exponentially large. [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi (2023)]] eliminates this by using $C(A)$ instead.

- **$g'$ parameter.** The quantity $g' = \max_{t,n} \|x^{(n+1)}(t)\|$ measures the smoothness of the solution. For well-behaved ODEs, $g' \le \kappa_V(\|\gamma\| + 2\tau\|f\|)$, but in general it can be hard to bound. The Berry-Costa Dyson series approach avoids this parameter entirely.

- **BVP scaling is worse.** The BVP complexity has $\|A\|^4 T^4$ instead of $\|A\|T$ because there's no subinterval decomposition — the entire interval must be covered by a single Chebyshev expansion. This means $n$ must be $O(\|A\|T)$ times larger, which cascades through the condition number.

- **Output is a quantum state.** Same caveat as all QLSA-based approaches.

- **Non-diagonalisable $A(t)$ degrades to $\mathrm{poly}(1/\varepsilon)$.** The $\mathrm{polylog}$ scaling requires diagonalisability. For non-diagonalisable matrices, the condition number analysis breaks down.

## Reusable ideas

1. [[Chebyshev Spectral Discretisation for Quantum ODE Solvers]] — approximate the ODE solution globally using a truncated Chebyshev series at Chebyshev-Gauss-Lobatto nodes, producing a linear system for the coefficients solvable by QLSA. The key win: spectral convergence gives $\mathrm{polylog}(1/\varepsilon)$ scaling for smooth solutions.

2. [[Quantum Boundary Value Problem via Spectral Method]] — the spectral approach naturally handles BVPs: replace the initial condition row in the linear system with the boundary condition $\alpha x(0) + \beta x(T) = \gamma$. No subinterval decomposition needed (though at the cost of higher $T$-dependence).

## References within this paper

- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum ODE algorithm; history-state + QLSA, $\mathrm{poly}(1/\varepsilon)$
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — Taylor series approach, $\mathrm{polylog}(1/\varepsilon)$ for time-independent case
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the original QLSA
- Childs-Kothari-Somma (2017) — high-precision QLSA used as subroutine
- Leyton-Osborne (2008) — quantum algorithm for nonlinear ODEs with exponential-in-$T$ complexity
- Gheorghiu (2007) — convergence theory of spectral methods (Lemma 1)
- Shen-Tang-Wang (2011) — spectral methods reference ($C^\infty$ convergence, Lemma 2)
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] — related work on FEM for PDEs (cited as Ref. [16] is Clader et al., not Montanaro-Pallister, but the broader programme of quantum methods for PDEs connects)

## Cross-links

### Paper notes
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — predecessor: finite-difference approach to quantum ODE solving
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — predecessor: Taylor series for time-independent case
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — later approach to time-dependent ODEs that avoids the spectral method machinery
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — same authors (Childs, Liu) later apply quantum ODE solvers to nonlinear equations via Carleman linearisation
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] — eliminates $\kappa_V$ dependence via $C(A)$, handles non-diagonalisable matrices
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — QLSA subroutine
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — later optimal QLSA
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]] — complementary spatial discretisation approach (FEM vs spectral)
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — preconditioning for QLSA

### Trick cards
- [[Chebyshev Spectral Discretisation for Quantum ODE Solvers]] — the core technique *(new)*
- [[Quantum Boundary Value Problem via Spectral Method]] — BVP extension *(new)*
- [[History-State Padding for Final-Time Readout]] — padding trick used here
- [[History-State Linear System Encoding for ODE Trajectories]] — general framework
