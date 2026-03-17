
> **Tags:** #trick #dyson #time-ordering #LCU
> **Source:** arXiv:1805.00582

## What it does
Implements time-ordered integrals coherently inside an [[Linear Combination of Unitaries (LCU)|LCU]] circuit for truncated [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]] simulation. The ancilla registers hold time indices $t_1, \ldots, t_k$ for each order-$k$ term; the circuit ensures these satisfy $t_1 \le t_2 \le \cdots \le t_k$ before the [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] operations are applied.

## The two realizations

**Compressed-gap encoding:** Represent the ordered tuple by its gaps $\Delta_j = t_{j+1} - t_j$ (discrete increments). [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] the gap registers using an exponential-distribution state from the Childs–Cleve–Jordan–Yonge-Mallo construction, then recover absolute times by prefix sum. Naturally enforces monotonicity without sorting. Downside: omits repeated-time tuples, requiring a birthday-bound argument to control the resulting error. See [[Compressed Gap Encoding for Ordered Tuples]].

**Sort-based encoding:** [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] an unordered superposition over all $k$-tuples and apply a reversible sorting network (e.g. bitonic sort), storing comparator bits in ancilla for later uncomputation. Handles repeated times correctly at the cost of $O(K \log K)$ comparator gates. See [[Sorting-Based Time-Ordering for Dyson LCU]].

See [[Compressed Gap Encoding for Ordered Tuples]] and [[Sorting-Based Time-Ordering for Dyson LCU]] for details on each.

## When to use
Any [[Linear Combination of Unitaries (LCU)|LCU]] construction that requires a coherent superposition over ordered tuples of indices — [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]], certain Magnus-expansion implementations, or similar.

## Caveat
The time-register overhead ($O(K \log M)$ qubits for truncation order $K$ and discretization $M$) and the ordering circuit can dominate gate count at small $K$.

## Related notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Compressed Gap Encoding for Ordered Tuples]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Collision Budgeting in Discretized Ordered Integrals]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
