
> **Source:** Berry, Wan, Baczewski, Eklund, Tikku, Babbush, arXiv:2510.07380
> **Tags:** #trick #space-filling-curve #FMM #data-access #sorting

## What it does
Guarantees that every box in the FMM interaction list is within a bounded distance in at least one of $2^d$ Morton orderings, enabling fixed-location access to all interaction-list data without quantum addressing.

## The trick
A single Morton (Z-order) curve through a $d$-dimensional grid does **not** guarantee that spatially nearby boxes are close in the 1D ordering — the interaction list (up to 189 boxes in 3D) can scatter across the entire curve. This makes it impossible to access all interaction-list data with a single linear sweep over sorted particle registers.

Fix: use $2^d$ Morton orderings, each shifted by a vector $z \in \{0, 2\}^d$. For each shift, add $z$ to all box coordinates (mod grid size), re-interleave the bits, and re-sort.

**Lemma 1** proves: for any box $p$ and any box $q$ satisfying $|\lfloor p_j/2 \rfloor - \lfloor q_j/2 \rfloor| \leq 1$ (i.e., $q$ is in $p$'s interaction list or neighbourhood), there exists a shift $z \in \{0, 2\}^d$ such that

$$|M(p + z \bmod 2^n) - M(q + z \bmod 2^n)| \leq 4^d - 1$$

where $M$ is the Morton ordering function. In 3D, this means $|M(p+z) - M(q+z)| \leq 63$.

The proof works by showing that for the right choice of shift, $p+z$ and $q+z$ land in the same level-$(\ell-2)$ box (same top bits after shifting), so the Morton ordering groups them within $4^d - 1$ positions.

In the quantum algorithm, for each of the $2^d = 8$ shifts at each tree level:
1. Add $z$ to box coordinates
2. [[Quantum Sorting for Fixed-Location Data Access|Sort]] into Morton order
3. Linear sweep (Algorithm 4) copies charge/multipole data from up to $K = 4^d - 1$ neighbours
4. Compute interaction-list potentials using the copied data
5. Unsort, subtract $z$

## When to reach for it
- Quantum hierarchical algorithms (FMM, Barnes-Hut, panel methods) where you need to access spatially nearby data without QRAM
- Any setting where a single space-filling curve doesn't provide strong enough locality guarantees
- Classical parallel FMM implementations that need cache-friendly interaction-list traversal

## Complexity
$2^d$ sorts per tree level, $O(\log N / d)$ tree levels, each sort costs $O(\eta \log \eta \log N)$ gates. Total sorting cost: $O(\eta \log \eta \log^2 N)$. The $2^d$ factor is 8 in 3D — a constant, but not a small one.

## Caveat
The $2^d$ orderings provide more coverage than strictly necessary — many boxes appear within range in multiple orderings, so duplicate interactions must be filtered. The paper notes that alternative approaches (Hilbert orderings, different shift strategies) might reduce this redundancy. Morton ordering is used here because it maps trivially to bit interleaving, making the quantum implementation straightforward.

## Related notes
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]
- [[Quantum Sorting for Fixed-Location Data Access]]
- [[Per-Particle Box Registers (Avoiding Quantum Addressing)]]
- [[Forward-Backward Sweep for Hierarchical Aggregation]]
- [[Sorting Networks as Quantum Control-Flow Compilers]]
