# Three-Way Grover Analysis for Entangling Oracle Errors

> **Source:** Bulger, arXiv:quant-ph/0507193
> **Tags:** #trick #Grover #error-analysis #oracle #optimization

## What it does

Analyses a Grover search when the marking predicate is implemented by an imperfect coherent subroutine whose errors can entangle the work space with the search register.

## The trick

Ordinary Grover analysis partitions the search space into two sets: unmarked and marked. If the predicate is produced by an approximate coherent computation, there can be a third set near the decision boundary where small subroutine errors change the mark.

Partition the initial points as:

$$
S_\alpha = \text{definitely unmarked}, \qquad
S_\gamma = \text{definitely marked}, \qquad
S_\beta = \text{boundary / entangling region}.
$$

For Bulger's basin-hopping case, $S_\beta$ consists of starting points where local descent with gradient errors bounded by $\delta$ can end both above and below threshold $Y$.

The Grover iterate no longer stays in the clean span of marked and unmarked uniform states. But if the boundary is small, the leakage is perturbative. In the regime

$$
N_\beta \ll N_\gamma \ll N_\alpha,
$$

using the usual Grover iteration scale

$$
k \approx \frac{\pi}{4}\sqrt{\frac{N}{N_\gamma}}
$$

still succeeds with error of order

$$
O\!\left(\sqrt{\frac{N_\beta}{N_\gamma}}\right).
$$

## When to reach for it

- Grover or amplitude-amplification routines whose oracle calls a numerical subroutine.
- Threshold tests on approximate real-valued quantities.
- Coherent optimisation routines where rounding, gradient estimation, or finite precision can flip the predicate near a boundary.
- Any setting where “marked” is stable away from a boundary but ambiguous close to it.

## Complexity

The Grover iteration count remains the same as the ideal marked-set size would suggest, provided the boundary set is small relative to the marked set. The extra cost is in setting the subroutine precision high enough that large errors have total amplitude $o(1)$ over all oracle calls.

For $(2r+1)L$ approximate gradient estimates, Bulger bounds the large-error state perturbation by

$$
(2r+1)L\epsilon.
$$

## Caveat

This is not a black-box fault-tolerance theorem for Grover oracles. The argument uses structure: the ambiguous region must shrink with precision, and the boundary count $N_\beta$ must be much smaller than the marked count $N_\gamma$. If the threshold sits on a broad plateau, the bound gives no comfort.

## Related notes

- [[Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005) — Paper Notes]]
- [[Quantum Basin Hopping via Basin Membership Oracles]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]
- [[Randomised Iteration Count for Grover Search]]
