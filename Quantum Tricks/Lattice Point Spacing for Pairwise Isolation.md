# Lattice Point Spacing for Pairwise Isolation

> **Source:** Regev, arXiv:cs/0304005
> **Tags:** #trick #lattice-problems #quantum-reduction #state-preparation

## What it does
Subsamples lattice points so that spatial cells contain at most one pair of points whose difference is the shortest vector, enabling collapse to a clean two-point superposition upon measurement.

## The trick
Create a superposition over lattice points $f(t, \bar{a}) = (a_1 p + tm)\bar{b}_1 + \sum_{i=2}^n a_i \bar{b}_i$ where $t \in \{0,1\}$, $\bar{a} \in \{0, \ldots, M-1\}^n$, and $p$ is a prime larger than $n^{2+2f}$. The factor $p$ in the first coordinate "spaces out" lattice points along the shortest vector direction: two points with the same $t$ value differ by a multiple of $p \cdot \bar{u}$, which is too long to fit in a single cell. Only points with $t=0$ and $t=1$ can share a cell, and their difference is exactly $\bar{u}$.

After partitioning space into cells (cubes or balls of radius proportional to $\|\bar{u}\|$) and measuring which cell a point falls in, the superposition collapses to $\frac{1}{\sqrt{2}}(|0, \bar{a}\rangle + |1, \bar{a}'\rangle)$ with $\bar{a}' - \bar{a}$ encoding the shortest vector coordinates.

## When to reach for it
When you need to isolate pairs of structured points from a dense lattice (or any discrete set with a distinguished direction/periodicity). The spacing trick converts a many-point superposition into a two-point one, which is the input format for the dihedral coset problem.

## Complexity
Preparing the superposition is polynomial in $n$ (standard basis enumeration + LLL-reduced basis arithmetic). The measurement is a single-shot collapse. The overhead is in the $O(pn^2)$ guesses for the parameters $l, m, i_0$.

## Caveat
Requires the uniqueness promise — the shortest vector must be separated from all non-parallel vectors by a factor of $\Theta(n^{1/2+2f})$. If multiple short vectors exist at comparable lengths, cells may contain unwanted points and the collapse is corrupted.

## Related notes
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Ball vs Cube Partition for Tighter Lattice Guarantees]]
- [[Phase Recovery via Subset Sum Oracle]]
