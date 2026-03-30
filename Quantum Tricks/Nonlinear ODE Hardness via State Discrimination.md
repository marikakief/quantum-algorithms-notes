# Nonlinear ODE Hardness via State Discrimination

> **Source:** Liu, Kolden, Krovi, Loureiro, Trivisa, Childs, arXiv:2011.03185 (Theorem 2)
> **Tags:** #trick #lower-bound #nonlinear-ODE #state-discrimination #Helstrom-bound

## What it does
Proves that simulating quadratic ODEs with strong nonlinearity ($R \ge \sqrt{2}$) requires time exponential in $T$ for any quantum algorithm.

## The trick

Construct a 2D quadratic ODE $\mathrm{d}u_j/\mathrm{d}t = -u_j + Ru_j^2$ (for $j = 1,2$) with $R \ge \sqrt{2}$. The analytic solution is $u_j(t) = 1/(R - e^t(R - 1/u_j(0)))$. For initial values above the threshold $1/R$, the solution grows toward a finite-time singularity; below, it decays to zero.

Now pick two initial quantum states with overlap $1 - \varepsilon$:
- $|\phi(0)\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$ — symmetric, both components at $1/\sqrt{2}$
- $|\psi(0)\rangle$ — slightly tilted, components $(v_0, w_0)$ with $w_0 > 1/\sqrt{2} > v_0$

The nonlinear dynamics amplifies the asymmetry: $w(t)$ grows while $v(t)$ shrinks (or grows slower). After time $T = O(\log(1/\varepsilon))$, the overlap drops to a constant ($\le 3/\sqrt{10}$). Since distinguishing states with overlap $1-\varepsilon$ requires $\Omega(1/\varepsilon)$ copies (Helstrom bound), simulating this ODE requires $\Omega(1/\varepsilon) = \Omega(2^T)$ queries to the initial state oracle.

The argument is information-theoretic: it applies to *any* quantum algorithm for the ODE, not just Carleman-based approaches.

## When to reach for it

- Proving hardness of simulating other nonlinear dynamical systems: the template is "construct an instance where small input differences amplify to large output differences in logarithmic time"
- Understanding the complexity landscape of quantum simulation problems — identifying which parameter regimes are tractable vs. intractable
- The specific construction works whenever you have a scalar ODE $\dot{u} = -u + Ru^2$ with $R \ge \sqrt{2}$ and an unstable fixed point at $1/R$

## Complexity

The lower bound is $\Omega(2^T)$ on the number of queries to the initial state oracle. Combined with the upper bound of $O(T^2 \cdot \mathrm{poly}(\log))$ for $R < 1$, this establishes a sharp phase transition in complexity as a function of $R$.

## Caveat

- The gap $1 \le R < \sqrt{2}$ remains open. It's unknown whether problems in this range are tractable or intractable.
- The lower bound is worst-case: specific ODE instances with $R \ge \sqrt{2}$ might still be solvable efficiently if they have additional structure.
- The argument relies on the ODE having an unstable fixed point. Dissipative systems with $R < 1$ don't have this feature — all trajectories converge.

## Related notes
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — source paper
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the algorithm that works in the $R < 1$ regime
- [[Measurement-Induced Nonlinearity]] — related concept: nonlinearity from measurement enables computational power
