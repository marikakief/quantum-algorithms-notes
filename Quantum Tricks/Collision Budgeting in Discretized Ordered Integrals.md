
> **Tags:** #trick #error-analysis #dyson
> **Source:** arXiv:1805.00582, Section VI

## What it does
Controls the error introduced when the gap-encoding clock-preparation omits repeated-time tuples (tuples with $t_i = t_j$ for some $i \ne j$) from a discretized [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]].

## Why it arises
When using gap-encoding to [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] the time register (see [[Compressed Gap Encoding for Ordered Tuples]]), only strictly increasing tuples are prepared. After discretizing to $M$ time points, the true Dyson integral assigns nonzero weight to tuples with repeated indices. Omitting these introduces a bias proportional to the probability that any two of $k$ indices collide when drawn uniformly from $[M]$ — a birthday-type calculation.

For order-$k$ terms, the collision probability scales as $\binom{k}{2}/M$. Summed over the $K$ orders in the truncation, the total omission error is $O(K^2/M)$.

## The fix
Choose $M$ large enough that this error is below $\varepsilon/r$ (per segment, where $r$ is the number of segments). Combined with the discretization error from approximating continuous integrals with Riemann sums (which requires $M = \Omega(t\|\dot H\|_{\max}/(d\varepsilon H_{\max}))$), the binding constraint on $M$ is:

$$
M = \Theta\!\left(\frac{t\,(\|\dot H\|_{\max} + H_{\max}^2)}{d\,\varepsilon\,H_{\max}}\right).
$$

(The $H_{\max}^2$ term comes from collision budgeting; the $\|\dot H\|_{\max}$ term from discretization of the integral.)

## Practical effect
This increases $M$, which in turn increases the clock-register size ($O(K \log M)$ qubits) and the gate cost of the clock-preparation circuit. For Hamiltonians with large $H_{\max}$ or slowly varying norm, this can be a meaningful overhead.

## When collision budgeting is avoidable
If you use the sort-based clock preparation instead of gap encoding, repeated-time tuples are included automatically and no collision budget is needed.

## Related notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Compressed Gap Encoding for Ordered Tuples]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
