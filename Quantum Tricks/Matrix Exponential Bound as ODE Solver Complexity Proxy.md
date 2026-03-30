# Matrix Exponential Bound as ODE Solver Complexity Proxy

> **Source:** Krovi, arXiv:2202.01054 (Definition 5, Theorem 7)
> **Tags:** #trick #ODE #matrix-exponential #complexity-characterisation #log-norm #stability

## What it does
Replaces eigenvalue-based conditions (diagonalisability, spectral abscissa) with a single quantity $C(A) := \sup_{t \ge 0} \|e^{At}\|$ that directly characterises the complexity of quantum ODE solvers.

## The trick

For the ODE $\dot{x} = Ax + b$, the solution norm is bounded by $\|x(t)\| \le C(A)(\|x_0\| + T\|b\|)$. This means the condition number of the time-stepping linear system, the measurement probability, and ultimately the full query complexity all depend on $C(A)$ — not on the eigenvalue structure of $A$.

For the three regimes of interest:
- **$\mu(A) \le 0$ (negative log-norm):** $\|e^{At}\| \le e^{\mu t} \le 1$, so $C(A) = 1$. Best case.
- **$\alpha(A) < 0$ but $\mu(A) > 0$ (transient growth):** $\|e^{At}\|$ initially grows (up to $\sim e^{\mu t}$ for short $t$) then decays (at rate $\sim e^{\alpha t}$ for long $t$). The Kreiss matrix theorem bounds $C(A) \le ed\mathcal{K}(A)$.
- **$\alpha(A) \ge 0$ (unstable):** $C(A) = \infty$. The ODE problem is intractable.

For non-normal matrices, the gap between $\alpha$ and $\mu$ can be large (e.g., $A = \begin{pmatrix} -2 & 10 \\ 0 & -2 \end{pmatrix}$ has $\alpha = -2$ but $\mu \approx 3$). Previous algorithms depending on $\kappa_V$ (condition number of the diagonalising matrix) pay an exponential penalty for such matrices; the $C(A)$ characterisation avoids this entirely.

## When to reach for it

- Analysing quantum ODE solver complexity for non-normal or non-diagonalisable coefficient matrices
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]]: the Carleman block matrix is always non-normal (it's block-tridiagonal), and $C(A) \le N$ (the truncation order) is a clean bound
- Any quantum linear algebra primitive where you need to bound $\|f(A)\|$ and eigenvalue decomposition is either unavailable or gives loose bounds
- Comparing different ODE algorithms: $C(A)$ provides a unified complexity measure

## Complexity

The query complexity of the ODE solver is $O(g \cdot T\|A\| \cdot C(A) \cdot \mathrm{polylog}(\cdots))$ where $g = \max_t \|x(t)\|/\|x(T)\|$. For normal matrices with $\mu \le 0$, $C(A) = 1$ and this recovers optimal scaling. For Carleman systems, $C(A) = N = O(\log(1/\varepsilon)/\log(1/R))$.

## Caveat

- For general matrices, computing $C(A)$ exactly may be as hard as computing $\|e^{At}\|$ for all $t$. The Kreiss bound $C(A) \le ed\mathcal{K}(A)$ can be exponential in dimension.
- $C(A)$ is a worst-case-over-time quantity. If you only need the solution at time $T$ (not over all intermediate times), the relevant quantity might be $\|e^{AT}\|$ alone, which could be much smaller.
- Doesn't help if $\alpha(A) \ge 0$ — the problem is genuinely hard in that case.

## Related notes
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] — source paper
- [[Log-Norm Diagonal Preconditioning for Carleman Systems]] — uses $C(A)$ for Carleman matrices
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the main application context
- [[Preconditioning Quantum Linear Systems via Fast Inversion]] — related idea: modify the system to improve condition
