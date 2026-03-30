# Log-Norm Diagonal Preconditioning for Carleman Systems

> **Source:** Krovi, arXiv:2202.01054 (Lemma 16)
> **Tags:** #trick #Carleman-linearisation #preconditioning #log-norm #matrix-exponential

## What it does
Bounds the [[Matrix Exponential Bound as ODE Solver Complexity Proxy|matrix exponential]] of the Carleman block matrix by $N$ (the truncation order), even though the matrix is non-normal and potentially non-diagonalisable.

## The trick

The Carleman matrix $A$ from linearising $\dot{u} = F_2 u^{\otimes 2} + F_1 u + F_0$ has block-tridiagonal structure:
- Diagonal blocks: $A_j^{(1)} = \sum_{\ell=1}^j I^{\otimes(\ell-1)} \otimes F_1 \otimes I^{\otimes(j-\ell)}$
- Off-diagonal blocks: involving $F_0$ (lower) and $F_2$ (upper)

Define the diagonal preconditioner $D = \mathrm{diag}(I/1, I/2, \ldots, I/N)$ — the identity on each block scaled by $1/j$. Then:

1. The symmetrised form $Q = DA + A^\dagger D$ splits as $(DH_1 + H_1^\dagger D) + (D(H_0 + H_2) + (H_0 + H_2)^\dagger D)$
2. The diagonal part gives $\mu(DH_1) = \mu(F_1)$ (the log-norm of $F_1$ alone)
3. The off-diagonal part satisfies $\mu(D(H_0 + H_2)) \le \|F_0\| + \|F_2\|$
4. When $R < 1$ (i.e., $|\mu(F_1)| > \|F_0\| + \|F_2\|$), we get $\mu(Q) < 0$

Since $\|e^{At}\| \le \kappa(D) = N$ whenever $\mu(DAD^{-1})$ can be bounded through $Q$, this gives $C(A) \le N$.

The $1/j$ scaling is the natural choice: the $j$th Carleman block involves $j$ copies of $F_1$ in its diagonal contribution, so scaling by $1/j$ normalises this to $\mu(F_1)$ uniformly across blocks.

## When to reach for it

- Any time you need to bound $\|e^{At}\|$ for a block-structured matrix where the diagonal blocks have bounded log-norm but the off-diagonal coupling makes the full matrix non-normal
- Specifically in [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] for nonlinear ODEs — this is the standard way to show $C(A) \le N$
- More broadly: diagonal preconditioning to convert a log-norm bound on subblocks into a global bound

## Complexity

The preconditioning itself is "free" — it's an analysis tool, not something implemented in the quantum circuit. The bound $C(A) \le N$ feeds into the ODE solver complexity as a multiplicative factor.

## Caveat

- Requires $R < 1$, which is the same dissipativity condition as before. The preconditioning doesn't relax this requirement.
- The bound $C(A) \le N$ is tight for some Carleman systems — the transient growth of $\|e^{At}\|$ can genuinely reach $N$ before decaying.
- Only applies when $\mu(F_1) < 0$. If $F_1$ has $\mu(F_1) \ge 0$ (even if $\alpha(F_1) < 0$), this preconditioning doesn't give a useful bound. Handling transient growth at the $F_1$ level is an open problem.

## Related notes
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] — source paper
- [[Matrix Exponential Bound as ODE Solver Complexity Proxy]] — the quantity being bounded
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the context
- [[Preconditioning Quantum Linear Systems via Fast Inversion]] — different preconditioning for different purposes
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — the paper this improves on
