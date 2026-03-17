
> **Source:** Guang Hao Low and Nathan Wiebe, *Hamiltonian Simulation in the Interaction Picture*, arXiv:1805.00675  
> **Tags:** #trick #error-analysis #dyson #discretization

## Idea

When discretizing the time-ordered integrals in a Dyson expansion, the required number of time grid points $M$ depends on how fast the Hamiltonian is varying. A crude bound uses $\max_t \|\dot H_I(t)\|$, but this can be much larger than the *average* rate of variation. For the [[Interaction-Picture Norm Reduction|interaction picture]] Hamiltonian $H_I(t) = e^{iAt}Be^{-iAt}$, the derivatives involve commutators like $\|[A, B]\|$, $\|[A,[A,B]]\|$, etc., and these nested commutators may be small even when $\|A\|$ is large.

The paper uses this to argue that the discretization budget for $H_I(t)$ can be set using the commutator norms $\|[A,B]\|$, $\|[A,[A,B]]\|$, ... rather than $\|A\|\|B\|$. When $A$ and $B$ nearly commute (e.g. $A$ large and diagonal, $B$ off-diagonal but small), these commutators are small, giving a much finer budget than a worst-case derivative bound.

## Why "exponentially better with derivatives"

The abstract of 1805.00675 says the algorithm scales "exponentially better with derivatives of the time-dependent component." This refers to the fact that the Dyson truncation order $K$ needed for precision $\varepsilon$ scales as $O(\log(1/\varepsilon))$, not $O(1/\varepsilon)$, and that each derivative norm (commutator) enters multiplicatively in the [[Linear Combination of Unitaries (LCU)|LCU]] normalization rather than raising the required $K$.

## Caveat

This is a win only when the commutator norms are genuinely small — not just when $\|B\|$ is small (that's the norm reduction trick), but also when the derivatives of $H_I$ are small. If $[A, B]$ is large (e.g. $A$ and $B$ are both off-diagonal with no commutation structure), this gives nothing over a direct Dyson simulation.

## Related notes

- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Interaction-Picture Norm Reduction]]
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
