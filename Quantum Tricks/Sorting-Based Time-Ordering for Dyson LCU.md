
> **Tags:** #trick #dyson #LCU #time-ordering
> **Source:** arXiv:1805.00582

## What it does
Prepares a coherent superposition over time-ordered $k$-tuples by first preparing an unordered superposition and then sorting in superposition using a reversible sorting network. This is an ingredient in the [[Coherent Time-Ordering via Ancilla Clocks]] approach to [[Linear Combination of Unitaries (LCU)|LCU]]-based [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]] simulation.

## Steps
1. For each truncation order $k$, apply Hadamards to $k$ separate time registers, each of size $\log M$ bits, to get a uniform superposition over all $k$-tuples $(t_1, \ldots, t_k) \in [M]^k$.
2. Run a reversible sorting network (e.g. bitonic sort, AKS, or a practical $O(k \log^2 k)$ network). Each comparator $C_{ij}$ swaps two elements and records the comparison outcome in a dedicated ancilla bit for later uncomputation.
3. The output is a sorted tuple; uncomputation of the ancilla (via reverse sort) restores the ancilla to $|0\rangle$, making the operation unitary.
4. Feed the sorted tuple into the [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] operation.

Gate cost: sorting adds $O(K \log K \cdot \log M)$ gates for truncation order $K$ (the log K factor from the sorting depth), plus $O(K \log K)$ ancilla bits for comparator records.

## Advantages over gap encoding
- Handles repeated-time tuples correctly (no collision budget needed)
- Conceptually straightforward: just uniform superposition + standard reversible sort
- The error analysis for the time-ordered integral is cleaner

## Disadvantages
- More gates than [[Compressed Gap Encoding for Ordered Tuples|gap encoding]] (extra log $K$ factor)
- More ancilla qubits
- For large $K$, sorting network depth can dominate

## When to prefer this
When the collision error analysis for [[Compressed Gap Encoding for Ordered Tuples|gap encoding]] is cumbersome, or when $K$ is small enough that the sorting overhead is tolerable.

## Related notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Coherent Time-Ordering via Ancilla Clocks]]
- [[Compressed Gap Encoding for Ordered Tuples]]
- [[Collision Budgeting in Discretized Ordered Integrals]]
