
> **Tags:** #trick #data-aggregation #tree-algorithm #FMM
> **Source:** arXiv:2510.07380 (Quantum FMM, Berry, Wan, Baczewski, Tikku, Babbush), Section V.1

## What it does
Aggregates information up a tree (child → parent charges) without quantum addressing, using two linear sweeps over sorted particle registers.

## The trick
After sorting particles into Morton order, particles in the same box are contiguous. To compute a parent box's total charge from its children:

1. **[[Forward-Backward Sweep for Hierarchical Aggregation|forward sweep]]** ($j = 0$ to $\eta-2$): At each child-box boundary, copy the left child's accumulated charge into the right child's parent register. Within a child, propagate the copied value along.

2. **Backward sweep** ($j = \eta-2$ down to $0$): Same in reverse — copy right child's charge leftward across boundaries.

3. **Add**: $Q(j, \ell) \leftarrow Q(j, \ell) + Q(j, \ell+1)$. Each particle now has its parent box's total charge.

Empty boxes are handled automatically: if a child has no particles, there's no boundary crossing, and the charge stays zero.

## When to reach for it
- Hierarchical sum/aggregation on quantum data (FMM upward pass, tree reductions)
- Any bottom-up computation on sorted sequences where groups need aggregate statistics
- Replaces tree-recursive algorithms with linear sweeps (no random access needed)

## Complexity
$O(\eta)$ work per tree level, hence $O(\eta L)$ overall where $L$ = tree depth. In the FMM setting $L = O(\log N)$ for a regular octree, so total overhead is near-linear in $\eta$ up to logarithmic factors (plus arithmetic costs for the charge/multipole words).

## Caveat
Requires data in sorted order first. Information is replicated per-particle (each particle stores its box's aggregate), so register cost is $O(\eta \cdot L)$ where $L$ = tree depth.

## Related notes
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]
- [[Quantum Sorting for Fixed-Location Data Access]]
- [[Per-Particle Box Registers (Avoiding Quantum Addressing)]]
- [[Shifted Morton Orderings for Interaction List Access]]
