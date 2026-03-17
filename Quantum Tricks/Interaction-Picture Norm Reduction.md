# Interaction-Picture Norm Reduction

> **Source:** Guang Hao Low and Nathan Wiebe, *Hamiltonian Simulation in the Interaction Picture*, arXiv:1805.00675  
> **Tags:** #trick #interaction-picture #hamiltonian-simulation

## Idea

Split the Hamiltonian as $H = A + B$ and move to the rotating frame with respect to $A$. The [[Interaction-Picture Norm Reduction|interaction picture]] Hamiltonian

$$
H_I(t) = e^{iAt} B e^{-iAt}
$$

has spectral norm $\|H_I(t)\| = \|B\|$ at every $t$. Running a [[Sorting-Based Time-Ordering for Dyson LCU|Dyson-series simulation]] of $H_I(t)$ therefore costs in terms of $\|B\|$ and its derivatives in the rotating frame, not $\|A+B\|$.

## When this actually helps

Both conditions must hold:

1. **$\|A\| \gg \|B\|$**: the large part is in $A$. If $A$ and $B$ have comparable norms, the frame change doesn't help.

2. **$e^{iAt}$ is fast-forwardable**: simulating $A$ for time $\sim \|B\|^{-1}$ is cheap. The canonical case is $A$ diagonal — then $e^{iAt}$ is a product of single-qubit phase gates and costs nothing beyond the FFT needed to switch basis. If implementing $e^{iAt}$ requires generic sparse simulation at cost $\tilde O(\|A\|/\|B\|)$, you give back exactly what you gained.

## What changes in the Dyson expansion

In the [[Interaction-Picture Norm Reduction|interaction picture]], the [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]] is an expansion in $B$ (not $A+B$). Derivatives of $H_I(t)$ involve commutators $[A, B]$, $[A,[A,B]]$, etc. The algorithm complexity depends on these commutator norms, which can be much smaller than $\|A\|$ even when $\|A\|$ is large — this is the "exponentially better with derivatives" claim in the abstract.

## Caveats

- This is a decomposition-dependent trick: the split $H = A + B$ must be chosen to make both conditions above hold.
- If $e^{iAt}$ is not fast-forwardable, you need an oracle for $A$-simulation, and the total cost includes both $A$ and $B$ contributions.
- The gain is exponential in some cases (diagonally dominant Hamiltonians) and modest in others.

## Related notes

- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Oracle Conjugation for Time-Dependent Encodings]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
