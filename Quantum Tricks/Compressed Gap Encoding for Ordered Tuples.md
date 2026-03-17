
> **Tags:** #trick #dyson #time-ordering #LCU
> **Source:** arXiv:1805.00582 (uses earlier construction from Childs–Cleve–Jordan–Yonge-Mallo)

## What it does
Encodes an ordered $k$-tuple of time indices $(t_1 \le \cdots \le t_k)$ via the gaps $(\Delta_1, \ldots, \Delta_k)$ where $\Delta_j = t_{j+1} - t_j$, rather than storing absolute times. Because gaps are nonneg and sum to $t$, any such gap tuple automatically gives a valid ordered sequence — no sorting required.

## The construction (1805.00582, Section V)
For each order $k$ in the Dyson expansion:
1. [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] a register encoding the gaps using the exponential-gap state of Childs et al. This produces a superposition over discrete gap sequences with probabilities matching the $1/k!$ Dyson coefficient.
2. Compute absolute times from gaps by prefix sum: $t_j = \sum_{i \le j} \Delta_i$.
3. Feed the resulting ordered time tuple into [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]].

Gate cost for clock preparation: $O(K(\log M + \log\log(r/\varepsilon)))$ where $M$ is the discretization count and $r$ the number of segments.

## Collision caveat
The construction samples from strictly increasing tuples (positive gaps). Tuples with $\Delta_j = 0$ (repeated times) are omitted. In the continuous integral these have measure zero, but after discretization to $M$ points, the probability weight on repeated-time tuples is $\Theta(k^2/M)$ per order-$k$ term — a birthday-type correction. This forces $M$ to scale with the derivative $\|\dot H\|_{\max}$ to keep the total omission error below budget. See [[Collision Budgeting in Discretized Ordered Integrals]].

## When to use over sorting
- When you want fewer ancilla qubits (no comparator ancilla needed)
- When the sorting-network overhead is the bottleneck
- When collision error can be controlled (moderate $k$, large enough $M$)

## Caveats
- Requires a correct birthday-bound analysis; underestimating the required $M$ causes bias errors
- The gap-state preparation circuit is more specialized than just Hadamards + sort

## Related notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Coherent Time-Ordering via Ancilla Clocks]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Collision Budgeting in Discretized Ordered Integrals]]
