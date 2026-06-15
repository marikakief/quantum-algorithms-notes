# Average-Case Lower Bounds via Collision Discovery

> **Source:** Montanaro, arXiv:1408.1816
> **Tags:** #trick #lower-bound #average-case #pattern-matching #query-complexity

## What it does

Proves classical average-case pattern-matching lower bounds by making the algorithm find a collision between queried pattern symbols and queried text symbols.

## The trick

Use injective random instances. Draw the text $T$ as a random permutation and draw $P$ either independently or as a planted sub-block of $T$.

Until the algorithm has seen a value that appears in both its pattern queries and its text queries, the two distributions are indistinguishable. If it makes $L$ pattern queries and $K$ text queries, a union bound gives collision probability at most roughly

$$
\frac{2KL}{n^d}.
$$

To distinguish with constant probability, it needs $KL=\Omega(n^d)$, hence

$$
K+L=\Omega(n^{d/2}).
$$

## When to reach for it

- Distributional lower bounds for matching or alignment tasks.
- Random injective-data problems where the only evidence of a planted structure is equality of sampled symbols.
- Proving that adaptivity does not help before the first collision appears.

## Complexity

The collision-discovery part gives $\Omega(n^{d/2})$ classical queries. Montanaro combines this with other terms to get

$$
\Omega\!\left((n/m)^d\log_q m+n^{d/2}/\sqrt{\log_q n}\right)
$$

for random $d$-dimensional pattern detection, up to the paper's block-size normalization.

## Caveat

This argument is classical. Quantum algorithms can search for collisions quadratically faster, which is exactly why the pattern-matching algorithm can beat the classical average-case lower bound.

## Related notes

- [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]
- [[Collision Problem Lower Bound via Polynomial Method]]
- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]
