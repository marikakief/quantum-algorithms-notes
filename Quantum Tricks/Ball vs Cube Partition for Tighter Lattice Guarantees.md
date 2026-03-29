# Ball vs Cube Partition for Tighter Lattice Guarantees

> **Source:** Regev, arXiv:cs/0304005
> **Tags:** #trick #lattice-problems #geometry #state-preparation

## What it does
Improves the uniqueness gap required for lattice-based quantum reductions from $n^{1+2f}$ (cubes) to $n^{1/2+2f}$ (balls), by using $n$-dimensional balls as spatial partition cells instead of axis-aligned cubes.

## The trick
When partitioning $\mathbb{R}^n$ to collapse a lattice superposition to two points, the partition cells must satisfy: (1) two points differing by the shortest vector $\bar{u}$ almost always share a cell, and (2) two points differing by any longer vector never share a cell.

**Cubes** with side length $\propto \|\bar{u}\|$: a lattice vector $\bar{v}$ can fit inside a cube if *any single coordinate* is small enough. A vector of length $c \cdot n \cdot \|\bar{u}\|$ could still have each coordinate of size $c \cdot \|\bar{u}\|$, so the uniqueness gap must be $\Omega(n)$ to guarantee separation.

**Balls** with radius $R = c \cdot n^{1/2+2f} \cdot \|\bar{u}\|$: a vector $\bar{v}$ only fits if $\|\bar{v}\| \leq 2R$, which is an $\ell_2$ condition. The key estimate (Claim 3.7): the relative volume of the intersection of two balls of radius $R$ shifted by $\bar{d}$ is $\geq 1 - O(\sqrt{n}\|\bar{d}\|/R)$. So two points differing by $\bar{u}$ share a cell with high probability ($1 - O(1/n^{2f})$), while any vector longer than $2R$ never fits.

The ball superposition is prepared via the [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph]] recursive bisection technique, which approximates the uniform distribution over grid points in a convex body using classical volume estimation as a subroutine.

## When to reach for it
Whenever a spatial partition argument in $\mathbb{R}^n$ incurs an extra factor of $\sqrt{n}$ or $n$ from $\ell_1/\ell_2$ conversion. Balls give $\ell_2$-natural guarantees without paying the $\ell_\infty$-to-$\ell_2$ penalty of axis-aligned cells.

## Complexity
Preparing the uniform superposition over grid points in an $n$-ball with radius $R \leq 2^{\text{poly}(n)}$ takes $\text{poly}(n)$ time using Grover-Rudolph + classical convex body volume estimation. The ball intersection guarantee adds no asymptotic cost to the reduction.

## Caveat
Requires the Grover-Rudolph state preparation, which itself relies on efficient classical algorithms for convex body volume approximation (Kannan-Lovász-Simonovits). The approximation error is exponentially small in $n$ (via Claim 3.8), so this is not a practical concern but adds theoretical complexity.

## Related notes
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]
- [[Lattice Point Spacing for Pairwise Isolation]]
