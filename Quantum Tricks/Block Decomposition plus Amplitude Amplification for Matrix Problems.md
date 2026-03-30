# Block Decomposition plus Amplitude Amplification for Matrix Problems

> **Source:** Ambainis, Buhrman, Høyer, Karpinski, Kurur, unpublished manuscript (2002)
> **Tags:** #trick #matrix-verification #amplitude-amplification #Grover #linear-algebra

## What it does
Converts a matrix verification problem ($AB = C$?) into a quantum search problem by first reducing dimensionality via random projection (Freivalds-style), then amplifying over blocks.

## The trick

**Step 1 — Block.** Partition the $n$ columns of $B$ and $C$ into $b$ blocks of $n/b$ columns each. Then $AB = C$ iff $AB_i = C_i$ for all $i \in [b]$.

**Step 2 — Randomize.** For each block $i$, pick a random vector $x$ and check $A(B_i x) = C_i x$. This is Freivalds' trick: a wrong block is detected with constant probability in $O(n \cdot n/b)$ time (matrix-vector products).

**Step 3 — Grover over rows.** The matrix-vector check $Ay = z$ reduces to $n$ inner product comparisons. Use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] over rows to find a violation in $O(\sqrt{n} \cdot n)$ steps instead of $O(n^2)$.

**Step 4 — Amplify over blocks.** Use [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] to search over the $b$ blocks for one that fails verification. This costs $O(\sqrt{b})$ iterations of the per-block test.

With $b = \sqrt{n}$: total $O(n^{1/4} \cdot n^{3/2}) = O(n^{7/4})$.

## When to reach for it

- Verifying a computation of the form $f(A, B) = C$ where $f$ is linear in one argument
- Any problem where Freivalds-style random projection reduces the check to a lower-dimensional problem, and you want a quantum speedup on top
- Matrix-vector verification as a subroutine in larger algorithms

## Complexity

$O(n^{7/4})$ for $n \times n$ matrix product verification. Later improved to $O(n^{5/3})$ by [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|quantum walk techniques]] (Buhrman-Špalek).

## Caveat

- The block size $b = \sqrt{n}$ is optimal for this two-level structure; going deeper (more recursion levels) doesn't improve the bound with this approach
- The classical subcomputation ($B_i x$ and $C_i x$) costs $O(n^{3/2})$ per block and dominates the per-iteration cost — the quantum speedup comes from searching over blocks and rows, not from faster classical linear algebra
- Superseded by quantum walk approaches that avoid the $O(n^2)$ bottleneck of loading submatrices

## Related notes
- [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
