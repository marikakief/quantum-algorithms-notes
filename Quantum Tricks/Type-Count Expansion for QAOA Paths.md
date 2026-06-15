# Type-Count Expansion for QAOA Paths

> **Source:** Boulebnane--Montanaro, arXiv:2208.06909
> **Tags:** #trick #QAOA #path-expansion #random-CSP #multinomial

## What it does
Reduces an average over many QAOA computational-basis paths to a multinomial sum over coordinate-wise empirical types.

## The trick
A $p$-layer QAOA success probability contains $2p+1$ computational-basis strings: $p$ from the bra side, one central string tested by the success projector, and $p$ from the ket side.

For each coordinate $i\in[n]$, collect its pattern across these strings:

$$
s_i=(y_i^{(0)},y_i^{(1)},\ldots,y_i^{(2p)})\in\{0,1\}^{2p+1}.
$$

Group coordinates by type

$$
n_s=|\{i:s_i=s\}|,
\qquad \sum_s n_s=n.
$$

The number of path tuples with those type counts is the multinomial coefficient

$$
\binom{n}{(n_s)_s}.
$$

For random $k$-SAT, the clause average depends only on combinations of the $n_s$, because a random clause is unsatisfied by several strings exactly when all those strings agree on the chosen variables with the right literal signs.

## Why it matters
This is the bridge from a $2^n$-dimensional QAOA amplitude calculation to a finite-dimensional variational/saddle-point problem for fixed $p$.

## When to reach for it
Use for average-case QAOA analyses on exchangeable random instances: random SAT, random XOR, random hypergraph problems, and spin-glass-like ensembles.

## Caveat
The number of types is $2^{2p+1}$, so the method is fixed-depth. It is not an efficient high-$p$ method without additional structure.

## Related notes
- [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]
- [[Saddle-Point Exponents for Generalized Multinomial Sums]]
