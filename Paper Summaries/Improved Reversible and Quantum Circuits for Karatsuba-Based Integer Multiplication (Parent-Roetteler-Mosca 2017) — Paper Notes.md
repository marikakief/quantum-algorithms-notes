> **Source:** Alex Parent, Martin Roetteler, Michele Mosca, *Improved reversible and quantum circuits for Karatsuba-based integer multiplication*, arXiv:1706.03419, TQC 2017
> **Links:** [arXiv](https://arxiv.org/abs/1706.03419)
> **Tags:** #quantum-arithmetic #reversible-computation #multiplication #Toffoli #pebble-games #space-time-tradeoff

---

## The computational problem

Compute the integer product $|x, y, 0\rangle \mapsto |x, y, xy\rangle$ for two $n$-bit integers on a quantum computer. Both $x$ and $y$ may be in superposition. The cost measures are:

- **Size:** total Toffoli gate count
- **Depth:** circuit depth (parallel execution)
- **Space:** total qubits (inputs + outputs + ancillae)

The schoolbook method gives $\text{Size}(n) = O(n^2)$, $\text{Space}(n) = O(n)$. Karatsuba's classical algorithm reduces size to $O(n^{\log_2 3}) \approx O(n^{1.585})$ by splitting into 3 half-size subproblems, but naively circuitizing the recursion also inflates space to $O(n^{\log_2 3})$.

## What the paper does

Reduces the space complexity of reversible Karatsuba multiplication from $O(n^{1.585})$ to $O(n^{1.427})$ while keeping the gate count at $O(n^{\log_2 3})$. The size increases by a constant factor less than 2. The depth of the parallel version rises to $O(n^{1.158})$ (from $O(n)$), and the total volume (depth $\times$ space) remains $O(n^{1 + \log_2 3})$.

The space improvement comes from a pebble game strategy on the complete ternary tree defined by the Karatsuba recursion. Rather than keeping all intermediate results alive simultaneously, the strategy computes subtrees, copies their outputs, and uncomputes them before proceeding to sibling subtrees.

## The algorithm / construction

### Karatsuba decomposition

Write $x = x_1 \cdot 2^{\lceil n/2 \rceil} + x_0$ and $y = y_1 \cdot 2^{\lceil n/2 \rceil} + y_0$. Then:

$$xy = 2^n A + 2^{\lceil n/2 \rceil} B + C$$

where $A = x_1 y_1$, $C = x_0 y_0$, and $B = (x_0 + x_1)(y_0 + y_1) - A - C$.

This requires 3 multiplications of $n/2$-bit numbers plus $O(n)$ additions. Recursing gives $O(n^{\log_2 3})$ total operations.

### Reversible Karatsuba circuit

Each level of the recursion proceeds as follows:

1. Compute $x_0 + x_1$ and $y_0 + y_1$ using in-place adders
2. Recursively compute $A = x_1 y_1$, $C = x_0 y_0$, and $(x_0 + x_1)(y_0 + y_1)$
3. Subtract $A$ and $C$ from the third product to obtain $B$
4. Compose $xy = 2^n A + 2^{\lceil n/2 \rceil} B + C$ via shifted additions

The circuit uses the [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro et al.]] in-place adder as the addition primitive. A controlled version of this adder (4$n$ Toffoli gates, T-depth $4n$) is used for the schoolbook base case.

The schoolbook base-case multiplier uses $n$ controlled additions for a total Toffoli count of $M_n = 4n^2 - 3n$.

### The compute-copy-uncompute pattern

Each Karatsuba level produces garbage (the intermediate products and partial sums). To clean this up, the circuit applies [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]]: run the Karatsuba circuit forward, copy the $2n$-bit result to a clean output register, then run the circuit in reverse. This doubles the gate count but returns all ancillae to $|0\rangle$.

Without further optimisation, this gives $K_n \leq 98 \cdot n^{\log_2 3}$ Toffoli gates and $O(n^{\log_2 3})$ ancilla qubits — the space scales the same as the gate count because all intermediate results at every recursion level are live simultaneously.

### Pebble game on the ternary tree

The Karatsuba recursion defines a complete ternary tree: each node (a multiplication of some bit-size) has 3 children (the three half-size multiplications). The paper models the question "when should we uncompute intermediate results?" as a reversible pebble game on this tree.

**Pebble game rules:** A pebble on vertex $v$ means the result of the corresponding multiplication is currently stored in memory. Placing a pebble requires all children to be pebbled. Removing a pebble requires the same. Each pebble represents $O(n/2^{\text{level}})$ qubits.

**The strategy:** Find a level $k$ in the tree such that the total size of all nodes above level $k$ roughly equals the size of each subtree rooted at level $k$. Process the level-$k$ subtrees sequentially: compute each subtree (pebble all its nodes), copy its root result, then uncompute (remove all pebbles within the subtree) before moving to the next sibling.

Setting the level $k \leq \frac{\log_2 n}{2 - \log_3 2} \approx 0.731 \log_2 n$ balances the two contributions and yields:

$$\text{Space}(n) = O\!\left(n^{1 + \frac{\log_2 3 - 1}{2 - \log_3 2}}\right) \approx O(n^{1.427})$$

### Adaptive cutoff

The recursion switches to schoolbook multiplication below a cutoff bit-size. A dynamic programming search over all possible splits and cutoff values finds that cutoff 11 is optimal for practical sizes ($n \leq 500$). The "adaptive Karatsuba" (aKara11) chooses optimal splits at each level rather than always halving, which matters when $n$ is not a power of 2.

## Key results

**Theorem (space-improved Karatsuba multiplication):**

| Metric | Kowada-Portugal-Figueiredo (2006) — sequential | Kowada-Portugal-Figueiredo (2006) — parallel | This paper |
|---|---|---|---|
| Size | $O(n^{\log_2 3})$ | $O(n^{\log_2 3})$ | $O(n^{\log_2 3})$ |
| Depth | $O(n^{\log_2 3})$ | $O(n)$ | $O(n^{1.158})$ |
| Space | $O(n^{\log_2 3})$ | $O(n^{\log_2 3})$ | $O(n^{1.427})$ |
| Volume | $O(n^{2\log_2 3})$ | $O(n^{1+\log_2 3})$ | $O(n^{1+\log_2 3})$ |

The reported Toffoli count for the forward multiplier comparison with adaptive cutoff at $n = 400$ is approximately 422,000 (vs. 640,000 for the schoolbook forward multiplier). Additional outer compute-copy-uncompute in a larger reversible algorithm must be counted separately.

**Generalisation:** For any recursion splitting into $a$ subproblems of size $n/b$ with $a > b$ (so the recursion tree is $a$-ary), the same pebbling strategy reduces space from $O(n^{\log_b a})$ to $O(n^{1 + \frac{\log_b a - 1}{2 - \log_a b}})$. For Karatsuba ($a = 3, b = 2$), the exponent drops from $\log_2 3 \approx 1.585$ to $1.427$.

## Comparison with prior work

| Method | Size (Toffoli) | Space (qubits) | Notes |
|---|---|---|---|
| Schoolbook | $O(n^2)$ | $O(n)$ | Simple, space-efficient |
| Kowada et al. (2006) | $O(n^{1.585})$ | $O(n^{1.585})$ | First reversible Karatsuba; space = size |
| **This paper** | $O(n^{1.585})$ | **$O(n^{1.427})$** | Pebble game decouples space from size |
| Kepley-Steinwandt (2015) | $O(n^{1.585})$ | — | Binary field $\mathbb{F}_{2^n}$, not integers |

The space improvement is modest in the exponent ($1.427$ vs. $1.585$), but it matters: for $n = 2048$ (relevant to Shor's algorithm for RSA), the exponent-only ratio $n^{1.585}/n^{1.427} \approx 3.3\times$ before constants, which is several thousand fewer logical qubits at this scale.

The volume is unchanged at $O(n^{1 + \log_2 3})$, so the space saving is paid for entirely by the depth increase from $O(n)$ to $O(n^{1.158})$.

## Limits / caveats

- The volume (depth $\times$ width) is not improved. The space saving comes at the cost of increased depth. If parallel execution time is the binding constraint, the Kowada et al. parallel version ($O(n)$ depth) is still better.
- The pebble game strategy is heuristic, not provably optimal. Better ternary-tree pebbling strategies might exist. Whether the ternary tree can be pebbled with $O(\log n)$ pebbles in polynomial steps (as the line graph can) is an open problem.
- For Shor's algorithm specifically, only multiplication by classical constants is needed ($|x\rangle|0\rangle \mapsto |x\rangle|cx \bmod N\rangle$), which has simpler circuits than the general $|x\rangle|y\rangle$ case treated here. The Karatsuba approach still applies but the constant factors differ.
- The adaptive cutoff optimisation (switching to schoolbook below bit-size 11) is found empirically and is specific to the Cuccaro-style adder. Different adder primitives (e.g., [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper's QFT adder]] or [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney's T-optimised adder]]) would change the crossover point.
- The T-count analysis is indirect: the paper counts Toffolis, each of which costs 7 T gates in the standard decomposition or 4 T gates using [[Temporary Logical-AND|temporary logical-ANDs]]. A direct T-count optimisation might yield different tradeoffs.

## Reusable ideas

1. [[Pebble Game Space Reduction for Recursive Circuits]] — Model a recursive circuit as a DAG pebble game to find when to uncompute intermediate results. Balancing subtree sizes against the remaining tree yields sublinear-in-size space usage.
2. [[In-Place Adders to Minimise Karatsuba Garbage]] — Using in-place (rather than out-of-place) adders at each Karatsuba level reduces the garbage per level, which compounds across all levels of the recursion.
3. [[Adaptive Cutoff for Recursive Quantum Circuits]] — Switch from a recursive algorithm to a simpler base-case algorithm when the subproblem size drops below an empirically optimised cutoff. The optimal cutoff depends on the relative constants of the two methods.

## References within this paper

- Kowada, Portugal, Figueiredo (2006) — "Reversible Karatsuba's algorithm." First reversible circuit for Karatsuba multiplication. This paper improves upon its space complexity.
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — The [[MAJ-UMA Decomposition for Reversible Adders|MAJ-UMA adder]] used as the addition primitive throughout.
- Bennett (1973, 1989) — [[Reversible Computation via Compute-Copy-Uncompute|Compute-copy-uncompute]] and pebble games for reversible space-time tradeoffs.
- Knill (1995) — "An analysis of Bennett's pebble game." Optimal strategies for line graphs.
- Komarath, Sarma, Sawlani (2016) — Pebbling of binary trees. Related but the ternary case here is different.
- Lange, McKenzie, Tapp (2000) — $O(\log n)$-pebble strategy for line graphs in $O(n \log n)$ steps. Shown as the "fractal" strategy in Figure 2.
- Selinger (2013) — Directional Toffoli gates (T-depth 1, 4 T gates each); used to implement controlled adders.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — Primary motivation: integer multiplication is the bottleneck in Shor's factoring algorithm.
- Kepley, Steinwandt (2015) — Similar Karatsuba approach for binary field multiplication ($\mathbb{F}_{2^n}$).

## Cross-links

### Paper notes
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — adder used as subroutine
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — alternative adder approach
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — T-count optimisation applicable to the Toffoli pairs in this circuit
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — primary application

### Trick cards
- [[Reversible Computation via Compute-Copy-Uncompute]] — the garbage cleanup pattern used at every recursion level
- [[MAJ-UMA Decomposition for Reversible Adders]] — the adder primitive
- [[Temporary Logical-AND]] — could halve the T-count of the Toffoli gates in this circuit
- [[Reversible Modular Exponentiation with Garbage Cleanup]] — the Shor subroutine that benefits from better multipliers
- [[Pebble Game Space Reduction for Recursive Circuits]] — extracted from this paper
- [[In-Place Adders to Minimise Karatsuba Garbage]] — extracted from this paper
- [[Adaptive Cutoff for Recursive Quantum Circuits]] — extracted from this paper
