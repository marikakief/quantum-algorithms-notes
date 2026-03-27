
> **Source:** D. W. Berry, J. Phys. A 47, 105301 (2014); arXiv:1010.2745
> **Tags:** #trick #history-state #ODE #linear-systems #HHL

## What it does
Converts a time-dependent ODE trajectory into a single quantum state by encoding the solution at all time steps simultaneously, then reformulates the ODE as a sparse linear system solvable by [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] or any quantum linear system solver.

## The trick

Given $\dot{x}(t) = Ax(t) + b(t)$ discretised into $N_t$ steps, build a quantum state that superposes the solution at every time step:

$$|\psi\rangle = \sum_{j=0}^{N_t} |t_j\rangle |x_j\rangle$$

This is a [[History-State Encoding with Unary Clock|Feynman clock / history state]] — the same structure Kitaev used for QMA-completeness, but now the "circuit" is the ODE propagator and the "gates" are the time-stepping recurrences.

The discretised ODE becomes a block-banded linear system $\mathcal{A}\vec{x} = \vec{b}$:
- First row: initial condition $x_0 = x_{\text{in}}$
- Middle rows: the stepping recurrence (Euler, multistep, etc.)
- Last rows: $x_{j+1} = x_j$ (padding — freezes the solution beyond $\Delta t$ so that measuring the time register in the second half always gives the final answer)

The padding trick is important: without it, the probability of measuring the final time is $1/(N_t+1)$. With padding (doubling the time register), it jumps to $\sim 1/2$.

## When to reach for it

- Solving any linear ODE system on a quantum computer — the go-to encoding
- PDEs after spatial discretisation (they become large sparse ODE systems)
- Any problem where you need a quantum state encoding a trajectory, not just a single time point
- As a reduction: "ODE → linear system → quantum linear system solver"

## Complexity

The condition number of $\mathcal{A}$ is $\kappa = O(N_t \kappa_V)$ where $\kappa_V$ is the condition number of the eigenvector matrix of $A$. With a $p$th-order multistep method, $N_t = O((\|A\|\Delta t)^{1+1/p}/\varepsilon^{1/p})$, giving total HHL cost $\sim \tilde{O}((\|A\|\Delta t)^{2+2/p})$.

## Caveat

The quadratic penalty in $\Delta t$ (relative to the $\Omega(\Delta t)$ lower bound) comes from HHL's $\kappa^2$ scaling. [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] fixed the precision dependence (poly → polylog in $1/\varepsilon$) but kept the $\Delta t^2$. [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry-Costa (2022)]] achieves optimal $\Delta t$ scaling by switching to Dyson series block-encodings within the same history-state framework, using the [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|optimal QLSA]].

Also: $A$ must satisfy a stability condition (eigenvalues in a left-half-plane wedge) for the multistep method to be $A(\alpha)$-stable.

## Related notes
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[History-State Encoding with Unary Clock]] — the underlying Kitaev clock
- [[History-State Padding for Final-Time Readout]] — the padding trick
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Taylor-Series-to-Linear-System Encoding]] — the improved encoding from Berry-Childs-Ostrander-Wang
