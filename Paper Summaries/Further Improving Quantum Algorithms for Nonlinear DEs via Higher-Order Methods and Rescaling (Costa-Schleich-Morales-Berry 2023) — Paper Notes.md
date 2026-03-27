> **Source:** Pedro C. S. Costa, Philipp Schleich, Mauro E. S. Morales, and Dominic W. Berry, *Further improving quantum algorithms for nonlinear differential equations via higher-order methods and rescaling*, arXiv:2312.09518, npj Quantum Information **11**, 141 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2312.09518) · [npjQI](https://doi.org/10.1038/s41534-025-01084-z)
> **Tags:** #nonlinear-ODE #Carleman-linearisation #quantum-algorithm #differential-equations #reaction-diffusion #PDE #rescaling #block-encoding

---

## The computational problem

Solve a system of nonlinear dissipative ODEs of the form

$$\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}t} = F_1 \mathbf{u} + F_M \mathbf{u}^{\otimes M},$$

where $\mathbf{u} \in \mathbb{R}^n$, $F_1 \in \mathbb{R}^{n \times n}$ is dissipative (all eigenvalues of $(F_1 + F_1^\dagger)/2$ have negative real part), and $F_M \in \mathbb{R}^{n \times n^M}$ encodes a polynomial nonlinearity of degree $M$. The dissipativity condition requires that the ratio

$$R := \frac{\|F_M\| \cdot \|\mathbf{u}_{\mathrm{in}}\|^{M-1}}{|\lambda_0|}$$

satisfies $R < 1$, where $\lambda_0$ is the eigenvalue of $(F_1 + F_1^\dagger)/2$ closest to zero. The task: output a quantum state encoding $\mathbf{u}(T)$.

The paper also applies this to a class of reaction-diffusion PDEs:

$$\partial_t u(x,t) = D\Delta u(x,t) + c\, u(x,t) + b\, u^M(x,t),$$

which after spatial discretisation becomes an ODE of the above form.

---

## What the paper does

Three improvements to quantum algorithms for nonlinear ODEs based on [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]]:

1. **Higher-order time evolution** — replaces the forward Euler method (used in Liu et al. 2021, Liu et al. 2023) with the truncated Taylor series ODE solver of Berry-Costa (2022, arXiv:2212.03544). This gives $\log(1/\varepsilon)$ dependence on error (instead of near-linear) and near-linear $T$ dependence (instead of $T^2$).

2. **Rescaling of the Carleman vector** — a variable substitution $\tilde{\mathbf{u}} = \mathbf{u}/\gamma$ that boosts the measurement probability of the solution component from exponentially small in $N$ (the Carleman truncation order) to $\Omega(1/N)$. Without this, the cost has a factor $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ that kills any quantum speedup for PDEs.

3. **Tighter Carleman error bounds** — explicit evaluation of nested integrals gives component-wise error bounds (Lemma 4) that decrease with the Carleman order $N$, unlike the global bound (Lemma 3) which saturates.

These together bring the oracle complexity for the ODE down to

$$O\!\left(\frac{1}{\sqrt{1 - R^{2/(M-1)}}} \cdot \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\|\mathbf{u}(T)\|} \cdot \lambda_{F_1} T N \log\frac{N}{\varepsilon} \log\frac{N\lambda_{F_1}T}{\varepsilon}\right),$$

with $N = O\!\left(\frac{(M-1)\log(1/\varepsilon)}{\log(1/R)}\right)$.

**My assessment:** This is a technically solid paper that fixes real problems with quantum Carleman algorithms. The rescaling trick is the most important contribution — it's what makes the PDE application viable at all, since without it the $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ factor makes the complexity superlinear in grid points, destroying any quantum advantage. The stability analysis for PDEs is honest about the tension between max-norm stability (needed for the PDE) and 2-norm rescaling (needed for quantum efficiency), which is a genuinely subtle point that earlier work glossed over.

That said, the $R < 1$ condition is restrictive. The approach works for weakly nonlinear, strongly dissipative systems. For the Navier-Stokes regime everyone actually cares about, $R$ is large and Carleman linearisation itself breaks down. The paper acknowledges this, but it's worth emphasising: this line of work gives quantum speedups for a class of problems where classical methods are already pretty good.

---

## The algorithm / construction

### Step 1: Carleman linearisation

The nonlinear ODE $\dot{\mathbf{u}} = F_1\mathbf{u} + F_M\mathbf{u}^{\otimes M}$ is lifted to an infinite-dimensional linear system by defining new variables $y_j = \mathbf{u}^{\otimes j}$ for $j = 1, 2, \ldots$. The derivative of $y_j$ involves $y_{j+M-1}$ (from the nonlinear term) and $y_j$ (from the linear term), giving an infinite block-banded linear ODE

$$\frac{\mathrm{d}\mathbf{y}}{\mathrm{d}t} = A_N \mathbf{y},$$

truncated at order $N$. The Carleman matrix $A_N$ has diagonal blocks $A_j^{(1)} = F_1 \otimes I^{\otimes(j-1)} + I \otimes F_1 \otimes I^{\otimes(j-2)} + \cdots$ and off-diagonal blocks $A_{j+M-1}^{(M)} = F_M \otimes I^{\otimes(j-1)} + \cdots$. The total dimension is $N_{\mathrm{tot}} = \sum_{j=1}^{N} n^j$.

### Step 2: Rescaling

Define $\tilde{\mathbf{u}} = \mathbf{u}/\gamma$ with $\gamma > 0$. This transforms the ODE to $\dot{\tilde{\mathbf{u}}} = F_1\tilde{\mathbf{u}} + \gamma^{M-1}F_M\tilde{\mathbf{u}}^{\otimes M}$. The Carleman vector becomes $\tilde{\mathbf{y}} = [\tilde{\mathbf{u}}, \tilde{\mathbf{u}}^{\otimes 2}, \ldots, \tilde{\mathbf{u}}^{\otimes N}]^T$, and the key point is that for $\gamma \geq \|\mathbf{u}_{\mathrm{in}}\|$ and dissipative dynamics ($\|\mathbf{u}(t)\| \leq \|\mathbf{u}_{\mathrm{in}}\|$), we get $\|\tilde{\mathbf{u}}(t)\| \leq 1$, which means $\|\tilde{\mathbf{y}}\|^2 \leq N$. The measurement probability for the solution component $\tilde{y}_1$ is then at least $1/N$ — no longer exponentially small.

The price: rescaling changes the off-diagonal blocks to $\gamma^{M-1}A_{j+M-1}^{(M)}$, which inflates the nonlinear part relative to the dissipative part. For the ODE solver to remain stable (non-positive logarithmic norm), we need $\gamma^{M-1} \leq |\lambda_0|/\|F_M\|$. This is exactly $\gamma \leq \|\mathbf{u}_{\mathrm{in}}\|/R^{1/(M-1)}$, which is satisfiable precisely when $R < 1$.

The amplitude amplification cost to extract the solution component is then

$$\frac{1}{\sqrt{1 - R^{2/(M-1)}}} \cdot \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\|\mathbf{u}(T)\|}$$

instead of $\|\mathbf{u}_{\mathrm{in}}\|^N / \|\mathbf{u}(T)\|$ without rescaling.

### Step 3: Higher-order ODE solver

The rescaled Carleman system $\dot{\tilde{\mathbf{y}}} = \tilde{A}_N \tilde{\mathbf{y}}$ is solved using the truncated Taylor series method of Berry-Costa (arXiv:2212.03544), which works as follows. The propagator $e^{\tilde{A}_N \Delta t}$ is approximated by a $K$th-order Taylor polynomial $W_K(\Delta t) = \sum_{\ell=0}^{K} (\tilde{A}_N \Delta t)^\ell / \ell!$, and the full evolution is encoded as a [[History-State Linear System Encoding for ODE Trajectories|history-state linear system]] solved by a [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|quantum linear system solver]].

The resulting complexity scales as $\lambda_{\tilde{A}_N} T \log(1/\varepsilon) \log(\lambda_{\tilde{A}_N} T/\varepsilon)$ oracle calls, where $\lambda_{\tilde{A}_N} = O(N\lambda_{F_1})$ is the block-encoding normalisation factor. The double-log structure gives near-linear time scaling with logarithmic error dependence — a major improvement over the $T^2 / \varepsilon$ scaling from forward Euler.

### Step 4: Block-encoding construction

The Carleman matrix $\tilde{A}_N$ is [[Block-Encoding Composition Algebra|block-encoded]] using the block encodings of $F_1$ and $F_M$ as building blocks. The off-diagonal blocks involve sums of tensor products of $F_1$ (or $F_M$) with identities, which can be expressed as [[Linear Combination of Unitaries (LCU)|LCUs]]. The normalisation factor is $\lambda_{\tilde{A}_N} \leq N\lambda_{F_1} + (N - M + 1)\gamma^{M-1}\lambda_{F_M}$, and under the stability condition this simplifies to $O(N\lambda_{F_1})$.

### Step 5: PDE application

For reaction-diffusion equations, $F_1 = DL_{k,d} + cI$ comes from a $k$th-order finite difference discretisation of the Laplacian ($2k+1$ stencil points), and $F_M$ is 1-sparse with entries $b$. The key subtlety: PDE stability is in terms of $\|\mathbf{u}\|_{\max}$ (Eq. 97: $\|\mathbf{u}_{\mathrm{in}}\|_{\max}^{M-1} |b/c| < 1$), but Carleman rescaling requires stability in terms of $\|\mathbf{u}\|_2$. Since $\|\mathbf{u}\|_2$ grows as $\sqrt{n}$ with the number of grid points $n$, this creates tension: more grid points can violate $R < 1$ even if the PDE itself is stable.

Higher-order spatial discretisation helps by reducing the number of grid points needed for a given accuracy. The spatial discretisation error scales as $O(n^{-(2k-1)/d})$, so higher $k$ reduces $n$, which in turn reduces $\|\mathbf{u}_{\mathrm{in}}\|_2$ and makes $R < 1$ easier to satisfy.

---

## Key results

| Result | Statement |
|---|---|
| ODE oracle complexity (Lemma 5) | $O\!\left(\frac{1}{\sqrt{1 - R^{2/(M-1)}}} \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\|\mathbf{u}(T)\|} \lambda_{F_1} T N \log\frac{N}{\varepsilon} \log\frac{N\lambda_{F_1}T}{\varepsilon}\right)$ |
| ODE initial state queries | $O\!\left(\frac{1}{\sqrt{1 - R^{2/(M-1)}}} \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\|\mathbf{u}(T)\|} \lambda_{F_1} T N^2 \log\frac{N}{\varepsilon}\right)$ |
| PDE oracle complexity (Corollary 6) | Same form with $\lambda_{F_1} \to dDn^{2/d} + |c|$ |
| Carleman order | $N = O\!\left(\frac{(M-1)\log(1/\varepsilon)}{\log(1/R)}\right)$ |
| Component-wise Carleman error (Lemma 4) | $\|\eta_1(t)\| \leq \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\gamma} R^{\lceil N/(M-1) \rceil} f_{1,\lceil N/(M-1)\rceil,M}(|\lambda_0|t)$ |
| Grid points for PDE | $n = \Omega\!\left(\left(\frac{C(u,k)}{\varepsilon(c + M|b|\|\mathbf{u}_{\mathrm{in}}\|_{\max}^{M-1})}\right)^{2d/(2(2k-1)+d)}\right)$ |

---

## Comparison with prior work

| | Liu-Kolden-Krovi et al. (2021) | Liu-An-[[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang et al. (2023)]] | Krovi (2023) | **This paper** |
|---|---|---|---|---|
| Time dependence | $T^2$ | $T^2$ | $T \cdot \text{poly}(\log)$ | $T \cdot \text{poly}(\log)$ |
| Error dependence | $\sim 1/\varepsilon$ | $\sim 1/\varepsilon$ | $\text{polylog}(1/\varepsilon)$ | $\log(1/\varepsilon)$ |
| Carleman order factor | $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ | $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ | $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ (unclear rescaling) | $O(1/\sqrt{1 - R^{2/(M-1)}})$ |
| $N$ scaling in complexity | $N^3$ | $N^3$ | poly$(N)$ | $N$ (oracle), $N^2$ (state prep) |
| Nonlinearity order | $M = 2$ | General $M$ | $M = 2$ | General $M$ |
| PDE grid-point scaling | superlinear in $n$ | $n^{4/d}$ | — | $n^{2/d}$ (for $d \geq 3$: sublinear) |
| Stability analysis | 2-norm | max-norm | 2-norm | Both (with tension identified) |

The most important comparison: without rescaling, prior work has a factor of $\|\mathbf{u}_{\mathrm{in}}\|^{2N}$ in the complexity. For a PDE with $n$ grid points, $\|\mathbf{u}_{\mathrm{in}}\|_2 \propto \sqrt{n}$, so this factor becomes $n^N$ — superlinear in $n$ and fatal for any quantum speedup. The rescaling eliminates this entirely.

---

## Limits / caveats

- **$R < 1$ is restrictive.** The dissipativity ratio $R$ must be strictly less than 1. For $R \geq \sqrt{2}$, Liu et al. (2021) proved the problem is BQP-hard to solve. The "interesting" range is $R \in (0,1)$, which corresponds to weakly nonlinear, strongly dissipative systems. Many scientifically relevant nonlinear PDEs (Navier-Stokes at high Reynolds number, turbulence) have $R \gg 1$.

- **Max-norm vs. 2-norm tension for PDEs.** PDE stability is in terms of $\|\mathbf{u}\|_{\max}$, but the rescaling needed for quantum efficiency is in terms of $\|\mathbf{u}\|_2$. Since $\|\mathbf{u}\|_2$ grows with $\sqrt{n}$, the 2-norm stability condition $R < 1$ can fail even when the PDE is perfectly stable. The paper is honest about this. In practice, you need the PDE to be "sufficiently dissipative" (strong enough $|c|$ relative to $|b|$) to compensate for the $\sqrt{n}$ growth.

- **Higher-order spatial discretisation helps but introduces its own problem.** The $k$th-order finite difference stencil with $k > 1$ breaks the $\|\cdot\|_\infty$-norm contractivity of the discretised Laplacian. The paper shows numerically that $\|e^{tDL_k}\|_\infty$ stays close to 1, but doesn't prove a clean bound. This makes the PDE error analysis partially heuristic for $k > 1$.

- **No driving term.** The analysis only covers $\dot{\mathbf{u}} = F_1\mathbf{u} + F_M\mathbf{u}^{\otimes M}$ without a constant forcing $F_0$. Including $F_0$ for general $M$ is more complicated because it generates a full polynomial of order $M$ after Carleman lifting. Left to future work.

- **Output is a quantum state.** Extracting classical information from $|\mathbf{u}(T)\rangle$ costs $O(n)$ in general. Only global observables (expectation values, norms) are efficiently accessible.

- **The Carleman approach is inherently limited.** Even with all these improvements, the method approximates the nonlinear dynamics by a truncated linear system. The truncation order $N$ grows as $\log(1/\varepsilon)/\log(1/R)$, so as $R \to 1$ the cost diverges. The method cannot handle genuine qualitative nonlinear behaviour (bifurcations, chaos, blowup).

---

## Reusable ideas

1. [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the general framework of lifting a polynomial nonlinear ODE to an infinite-dimensional linear system via Kronecker products of the solution vector, then truncating. Not new to this paper (Carleman 1932; Liu et al. 2021 for quantum), but this paper gives the clearest treatment of arbitrary-order nonlinearities.

2. [[Carleman Vector Rescaling for Measurement Probability Boost]] — the rescaling $\tilde{\mathbf{u}} = \mathbf{u}/\gamma$ that turns an exponentially-in-$N$ suppressed measurement probability into $\Omega(1/N)$. The core observation: choosing $\gamma \geq \|\mathbf{u}_{\mathrm{in}}\|$ with dissipative dynamics ensures $\|\tilde{\mathbf{u}}(t)\| \leq 1$, which bounds the Carleman vector's norm by $N$.

3. [[Norm Tension Between PDE Stability and Quantum Rescaling]] — the max-norm vs. 2-norm problem that arises when discretising a PDE for Carleman linearisation: PDE stability is in $\|\cdot\|_{\max}$, but quantum rescaling needs $\|\cdot\|_2$, and $\|\cdot\|_2 \propto \sqrt{n}$ defeats stability as $n$ grows. Motivates higher-order spatial discretisation to minimise $n$.

---

## References within this paper

- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — the original quantum ODE algorithm via [[History-State Linear System Encoding for ODE Trajectories|history state]] + [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — Taylor-series-based ODE solver with exponentially improved precision
- Berry-Costa (2022, arXiv:2212.03544) — the truncated Taylor series ODE solver used as the time-evolution subroutine; not yet in the vault
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa-An-Sanders-Su-Babbush-Berry (2022)]] — the optimal quantum linear system solver
- Liu-Kolden-Krovi-Loureiro-Trivisa-Childs (2021) — first quantum Carleman algorithm (quadratic nonlinearity, forward Euler)
- Liu-An-Fang-Wang-Low-Jordan (2023) — nonlinear reaction-diffusion via Carleman with max-norm stability; the direct predecessor this paper improves on
- Krovi (2023, arXiv:2202.01054) — improved quantum algorithms for linear and nonlinear DEs; mentions rescaling but implementation unclear
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — alternative time-marching approach for linear ODEs
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]] — near-optimal linear ODE solver via LCHS
- Carleman (1932) — the original linearisation technique

---

## Cross-links

### Paper notes
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — foundational; history-state encoding for linear ODEs
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — Taylor-series linear ODE solver that this paper's time-evolution subroutine builds on
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — optimal QLSP solver used as subroutine
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — constant-factor improvements to the QLSP solver
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] — alternative ODE approach; different trade-offs (no linearisation needed, but $O(T^2)$ matrix queries)
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]] — near-optimal linear non-unitary dynamics
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL; the starting point for all QLSP-based approaches
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — exponential speedup for a *linear* classical dynamics problem; shows what's achievable without nonlinearity
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — time-dependent linear ODE solver that this paper extends from linear to nonlinear dynamics via Carleman linearisation
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — later Costa/Berry work pushing adiabatic discretisation further; same broader program of reducing simulation overheads in differential-equation-style algorithms

### Trick cards
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]]
- [[Carleman Vector Rescaling for Measurement Probability Boost]]
- [[Norm Tension Between PDE Stability and Quantum Rescaling]]
- [[History-State Linear System Encoding for ODE Trajectories]] — the encoding used for the linearised system
- [[Taylor-Series-to-Linear-System Encoding]] — the time-evolution approach
- [[Block-Encoding Composition Algebra]] — used for constructing block encodings of $\tilde{A}_N$
- [[Linear Combination of Unitaries (LCU)]] — underlying primitive for block-encoding the Carleman matrix
