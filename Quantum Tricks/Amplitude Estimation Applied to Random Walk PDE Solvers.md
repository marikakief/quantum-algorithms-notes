# Amplitude Estimation Applied to Random Walk PDE Solvers

> **Source:** Linden, Montanaro, Shao, arXiv:2004.06516 (2022)
> **Tags:** #trick #amplitude-estimation #random-walk #PDE #heat-equation #Monte-Carlo

## What it does
Provides a quadratic quantum speedup for computing functionals of PDE solutions (e.g., total heat in a region) by applying [[Amplitude Amplification and Estimation|amplitude estimation]] to the random walk representation of the PDE, bypassing the linear systems formulation entirely.

## The trick

Many linear PDEs have a probabilistic interpretation via the Feynman-Kac formula or related results. The heat equation $\partial_t u = \alpha \nabla^2 u$ connects to random walks: the solution $u(x,t)$ equals the expected value of $u_0$ at the endpoint of a Brownian motion started at $x$ and run for time $t$.

Classically, to estimate $\int_S u(x,t)\,dx$ to error $\epsilon$:
1. Simulate many random walks, each starting from a random point in $S$
2. Average the initial-condition values at the walk endpoints
3. By central limit theorem, $O(1/\epsilon^2)$ walks suffice

Quantumly:
1. Encode the random walk as a quantum circuit (each step is a reversible random operation on a position register)
2. Mark whether the endpoint lies in $S$
3. Apply [[Amplitude Amplification and Estimation|amplitude estimation]] to estimate the probability of landing in $S$ with the correct weight
4. $O(1/\epsilon)$ walk invocations suffice — quadratic speedup

The "fast" variant replaces step-by-step walking with direct sampling from the binomial distribution (since consecutive random walk steps on a grid are independent), reducing the per-walk cost.

## When to reach for it

- Computing functionals $\int_S u(x,t)\,dx$ of solutions to PDEs with random walk interpretations (heat equation, diffusion, Laplace, some Schrödinger equations)
- When the QLSA approach gives poor performance due to condition number issues — this method is independent of $\kappa$
- When you need a classical output (a number), not a quantum state
- Estimating partition functions or expectation values that reduce to random walk averages

## Complexity

$\tilde{O}(1/\epsilon)$ for the heat equation in a rectangular region (fast random walk variant). For general regions, $\tilde{O}(1/\epsilon^2)$ — matching the classical fast random walk but using less space (polylog vs. poly).

The cost is independent of the condition number of any linear system, and independent of the spatial dimension $d$ in the $\epsilon$-scaling (though other parameters may depend on $d$).

## Caveat

- Only works for PDEs with a random walk / probabilistic interpretation. Non-dissipative equations (wave equation, Schrödinger) and nonlinear PDEs typically lack this structure.
- The "fast" variant requires rectangular geometry with periodic boundaries. General domains force the slower step-by-step walk.
- The speedup is "only" quadratic — from $O(1/\epsilon^2)$ to $O(1/\epsilon)$. For problems where $\epsilon$ is not extremely small, the quantum overhead may not be worthwhile.
- Requires efficient reversible implementation of the random walk transition kernel.

## Related notes
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]]
