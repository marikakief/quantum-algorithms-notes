# Forward Euler to Linear System Reduction

> **Source:** Liu, Kolden, Krovi, Loureiro, Trivisa, Childs, arXiv:2011.03185
> **Tags:** #trick #ODE-discretisation #linear-systems #forward-Euler #QLSA

## What it does
Converts a linear ODE into a block-bidiagonal linear system solvable by a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|quantum linear system algorithm]], using forward Euler time-stepping.

## The trick

Given a linear ODE $\dot{\hat{\mathbf{y}}} = A(t)\hat{\mathbf{y}} + \mathbf{b}(t)$ on $[0, T]$, discretise into $m$ steps of size $h = T/m$:

$$\mathbf{y}_{k+1} = [I + A(kh)h]\mathbf{y}_k + \mathbf{b}(kh).$$

Stack all time steps into a single linear system $L|\mathbf{Y}\rangle = |\mathbf{B}\rangle$ where

$$L = \begin{pmatrix} I & & & \\ -[I+A(0)h] & I & & \\ & \ddots & \ddots & \\ & & -[I+A((m-1)h)h] & I \end{pmatrix}$$

is block lower-bidiagonal. The right-hand side encodes the initial condition and forcing terms. Add $p$ extra "padding" time steps with identity propagation to boost the probability of measuring the final time slice (the [[History-State Padding for Final-Time Readout|history-state padding trick]]).

The condition number of $L$ is $\kappa \le 3(m+p+1)$, provided the time step satisfies a CFL-like stability condition ensuring $\|I + A(t)h\| \le 1$ for all $t$. This is the key point: forward Euler with a dissipative system gives a well-conditioned linear system whose condition number grows only linearly in the number of time steps.

## When to reach for it

- Solving linear ODEs on a quantum computer when you need a simple, analysable time discretisation
- When the system matrix $A$ is dissipative ($\operatorname{Re}(\lambda) < 0$ for all eigenvalues) — this is needed for the stability condition
- As the inner step of [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] for nonlinear ODEs
- When you want explicit control over the condition number (unlike implicit methods where $\kappa$ analysis is harder)

## Complexity

The linear system has dimension $(m+p+1)\Delta$ where $\Delta$ is the spatial dimension. The number of time steps is $m = O(T/h)$, and the time step must satisfy $h \le 1/(N\|F_1\|)$ (for real eigenvalues) or a more restrictive condition involving the imaginary parts. The condition number is $O(m)$, so the [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] cost scales as $O(s \cdot m \cdot \mathrm{polylog}(m\Delta/\varepsilon))$.

The global Euler error is $O(Mh T)$ where $M = \max_t \|\hat{\mathbf{y}}''(t)\|$, giving first-order convergence. This means $h = O(\varepsilon / (N^{2.5}T M'))$ for error $\varepsilon$, and the total number of steps is $m = O(T^2 N^{2.5} M'/\varepsilon)$ — the source of the $T^2/\varepsilon$ scaling in the original Carleman algorithm.

## Caveat

- First-order method: gives $T^2/\varepsilon$ scaling, which is the bottleneck of the original Carleman algorithm. Higher-order methods like the [[Taylor-Series-to-Linear-System Encoding|Taylor series approach]] achieve $T \cdot \mathrm{polylog}(1/\varepsilon)$.
- The CFL condition requires the time step to decrease as the system dimension grows (through $N$ dependence), which increases the number of time steps.
- Only well-conditioned for dissipative systems. Non-dissipative systems ($\operatorname{Re}(\lambda) \ge 0$) can give $\|I + Ah\| > 1$, causing the condition number to blow up.

## Related notes
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — original paper
- [[History-State Linear System Encoding for ODE Trajectories]] — the general framework
- [[History-State Padding for Final-Time Readout]] — the padding trick for boosting measurement probability
- [[Taylor-Series-to-Linear-System Encoding]] — the higher-order replacement
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the context where this discretisation is applied
