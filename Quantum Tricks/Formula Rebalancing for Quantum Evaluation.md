
> **Source:** Bshouty, Cleve, Eberly (1991); Bonet & Buss (1994); applied in Ambainis, Childs, Reichardt, Špalek, Zhang (FOCS 2007)
> **Tags:** #trick #formula-evaluation #preprocessing #classical

## What it does
Transforms an arbitrary Boolean formula into a "balanced" equivalent formula with controlled depth, at the cost of a small size blowup — ensuring that the spectral gap of the formula's adjacency Hamiltonian is polynomially large.

## The trick

Given a NAND formula of size $N$, for any parameter $k \geq 2$, construct an equivalent formula with:

$$\text{depth} \leq (9 \ln 2) k \log_2 N, \qquad \text{size} \leq N^{1 + 1/\log_2 k}$$

The depth-size tradeoff is controlled by $k$. For quantum formula evaluation, the spectral gap of the walk Hamiltonian depends on $\sigma_-(r) = O(\sqrt{\text{depth}})$ and $\sigma_+(r) = O(N \cdot \text{depth})$, so smaller depth → larger gap → fewer phase estimation queries.

Optimise: set $k = 2^{\sqrt{\log_2 N / 3}}$ to get total query complexity $N^{1/2 + O(1/\sqrt{\log N})}$.

## When to reach for it
- Any formula evaluation algorithm whose complexity depends on the formula's depth or balance
- When you need the spectral gap of a formula tree to be polynomially large
- Classical preprocessing step — done once, independent of the input

## Complexity
- Classical preprocessing: $\mathrm{poly}(N)$
- Size blowup: $N \to N^{1 + 1/\log_2 k}$ (sub-polynomial for large $k$)
- Depth reduction: from up to $N$ down to $O(k \log N)$

## Caveat
The rebalancing is a purely classical transformation — it produces a larger but shallower formula. The size blowup means the quantum algorithm evaluates more input bits, but the total query count still decreases because the spectral gap improves faster than the size grows.

Not needed for already-balanced formulas (balanced binary trees, constant-depth formulas).

## Related notes
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Szegedy Walk for Formula Evaluation]]
