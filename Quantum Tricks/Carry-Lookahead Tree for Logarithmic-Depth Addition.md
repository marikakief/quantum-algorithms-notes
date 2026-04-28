# Carry-Lookahead Tree for Logarithmic-Depth Addition

> **Source:** Draper, Kutin, Rains, Svore, arXiv:quant-ph/0406142
> **Tags:** #trick #quantum-arithmetic #carry-lookahead #depth-optimised #Toffoli #adder

## What it does

Computes all $n$ carry bits in $O(\log n)$ depth by organising the carry computation as a binary tree of interval merges, rather than a sequential ripple.

## The trick

Assign each bit position a carry status: **kill** ($a_i = b_i = 0$), **generate** ($a_i = b_i = 1$), or **propagate** ($a_i \neq b_i$). Encode as two bits: $p[i, j]$ (propagate across interval) and $g[i, j]$ (generate within interval). Merge adjacent intervals via:

$$p[i, j] = p[i, k] \wedge p[k, j]$$
$$g[i, j] = g[k, j] \oplus (g[i, k] \wedge p[k, j])$$

Each merge costs one Toffoli. The carry bits are $c_j = g[0, j]$.

The quantum circuit has four phases:

1. **P-rounds:** Build the propagate tree bottom-up ($\lfloor \log n \rfloor - 1$ rounds)
2. **G-rounds:** Build the generate tree bottom-up ($\lfloor \log n \rfloor$ rounds)
3. **C-rounds:** Propagate carries top-down using the generate tree
4. **P$^{-1}$-rounds:** Erase the propagate tree (run P-rounds in reverse)

P-round $t+1$ overlaps with G-round $t$ (they touch disjoint registers). C-round $t-1$ overlaps with P$^{-1}$-round $t$. This gives effective depth $\lfloor \log n \rfloor + \lfloor \log(n)/3 \rfloor + O(1)$.

Total cost: $4n - 3w(n) - 3\lfloor \log n \rfloor - 1$ Toffoli gates and $n - w(n) - \lfloor \log n \rfloor$ ancillae (for propagate scratch), where $w(n)$ is the Hamming weight of $n$.

## When to reach for it

- When circuit **depth** is the binding constraint rather than gate count or ancilla count — e.g., when trying to minimise the wall-clock time of a fault-tolerant computation
- As a subroutine in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] modular exponentiation to reduce the depth of each addition from $O(n)$ to $O(\log n)$
- In any circuit that performs many sequential additions where the total depth is dominated by the adder depth

## Complexity

- Depth: $O(\log n)$ (specifically $\lfloor \log n \rfloor + \lfloor \log(n)/3 \rfloor + 3$ for the carry computation alone)
- Gate count: $O(n)$ Toffolis
- Ancillae: $n - w(n) - \lfloor \log n \rfloor$ (for the carry computation); the full out-of-place adder needs the same, the in-place adder needs $\sim 2n$

## Caveat

The $O(n)$ ancillae are a significant cost. On surface-code hardware, idle ancillae have an [[Ancilla Opportunity Cost Analysis|opportunity cost]] in T-factory capacity. At large $n$ ($> 960$ in [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney's (2018)]] estimate), the ripple-carry adder with [[Temporary Logical-AND]] (depth $O(n)$, T-count $4n$) may be more efficient overall. A hybrid approach — carry-lookahead for low bits, ripple for high bits — can interpolate.

Also, applying [[Temporary Logical-AND|temporary logical-ANDs]] to the tree Toffolis gives T-count $\sim 12n$ (Brent-Kung variant), which is 3× worse than the ripple-carry adder's $4n$ T-count. The tree wins on depth, loses on T-count.

## Related notes

- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]]
- [[Two's-Complement Carry Erasure for In-Place Adders]]
- [[Phase Overlapping in Tree Circuits]]
- [[MAJ-UMA Decomposition for Reversible Adders]] — the ripple-carry alternative
- [[In-Place Majority for Carry Propagation]] — the MAJ gate that this tree replaces
- [[Temporary Logical-AND]] — applicable to paired Toffolis in the tree
- [[Ancilla Opportunity Cost Analysis]]
- [[Arithmetic in the Fourier Basis]] — competing $O(\log n)$-depth approach (QFT-based)
