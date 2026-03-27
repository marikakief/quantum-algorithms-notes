
> **Source:** Kivlichan, Wiebe, Babbush, Aspuru-Guzik, arXiv:1608.05696 (2017)
> **Tags:** #trick #swap-network #select-oracle #LCU #circuit-depth #adder #quantum-simulation

## What it does

Reduces the cost of implementing $\text{select}(V)$ — the oracle that applies $V_\chi$ to the system register based on the index $|\chi\rangle$ — from naively $O(\eta D a)$ queries (one per LCU term) to $O(1)$ queries. The trick is a binary-search-style swap tree that routes the target register to a fixed position, after which a single adder or potential oracle call suffices for all terms.

## The trick

**Setting:** The Hamiltonian LCU index is $|\chi\rangle = |i, n, j\rangle$ encoding particle index $i$, spatial dimension $n$, and finite-difference offset $j$. There are $2\eta D a$ kinetic terms total. Naively, implementing controlled-$A_j$ on particle $i$'s register costs one adder per $(i, n, j)$.

**Instead:**

1. **Swap to position 1** (for particle index $i$): controlled on the $k$-th bit of $i$, swap register $|x_{i'}\rangle$ with $|x_{i' - 2^{\lceil\log\eta\rceil - k}}\rangle$ for $i' \in [2^{\lceil\log\eta\rceil - k} + 1, 2^{\lceil\log\eta\rceil - k+1}]$. After $\lceil\log\eta\rceil$ stages (depth $O(\log\eta)$), particle $i$'s register is in position 1.

2. **Swap coordinate $n$ to the leading register** by analogous procedure (depth $O(\log D)$).

3. **Apply a single adder by $j$** to the leading register. The value $j$ is read from $|\chi\rangle$.

4. **Reverse the swap tree** to return all registers to original positions.

The entire circuit is controlled on $|\chi\rangle$, so it works on superpositions of indices:

$$\text{select}(V) |\chi\rangle |x\rangle = |\chi\rangle |x \oplus j\hat{e}_{i,n}\rangle$$

**For the potential terms:** same swap idea — route particles $i, j$ (from $|\chi\rangle = |i, j\rangle$) to registers 1 and 2, then apply a single pairwise potential oracle query. This reduces $O(\eta^2)$ distinct oracle calls to 1.

**Circuit costs:**
- Swap network gates: $O(\eta D \log(L/h))$ SWAP gates (each requires 3 CNOTs)
- Circuit depth: $O(\log(\eta D))$
- Queries: $O(1)$ adder calls + $O(1)$ potential oracle calls per $\text{select}(V)$

## When to reach for it

- Any LCU simulation where the Hamiltonian has a particle-index structure: identical terms acting on different registers indexed by $i$ (particles), $n$ (spatial dimensions), etc.
- When building $\text{select}(V)$ for [[Truncated Taylor Series Simulation]] on a first-quantized many-body system
- For lattice models where the same interaction type repeats across sites — a swap network can similarly reduce the oracle count
- When circuit depth is a constraint but total gate count is more flexible (swap networks trade gate count for depth)

## Complexity

- **Gate count:** $O(\eta D \log(L/h))$ for the swap network per $\text{select}(V)$ call
- **Circuit depth:** $O(\log \eta D)$ for the swap stages
- **Queries:** $\Theta(1)$ adders and potential oracle calls per $\text{select}(V)$
- **Overhead in full simulation:** $K = O(\log(r/\varepsilon)/\log\log(r/\varepsilon))$ calls to $\text{select}(W)$ per segment, each using $K$ calls to $\text{select}(V)$; total query count dominates at $O(Kr)$

## Caveat

- The swap network assumes the index register $|\chi\rangle$ stores $(i, n, j)$ as separate fields that can be decoded efficiently. If the indexing is more complex (e.g., irregular lattices), the swap routing becomes harder to implement.
- Controlled SWAP gates require a small constant overhead relative to single-qubit gates; for near-term devices with limited gate counts, this overhead matters.
- The analysis in Kivlichan et al. is in an oracle model where the adder circuit is one query and all other logic is free. In a gate-count model, the swap network's $O(\eta D \log(L/h))$ gates add real cost that can dominate for small systems.
- This technique applies to the particle-register structure specifically. The analogous CI representation trick in [[1-Sparse Hamiltonian Decomposition via Graph Coloring]] instead uses graph coloring to route row-to-column lookups, reflecting the different structure of the orbital-basis Hamiltonian.

## Related notes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[Real-Space Grid Encoding for Many-Body Simulation]]
- [[High-Order Finite Difference Kinetic Energy]]
- [[Truncated Taylor Series Simulation]]
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]] — analogous query-reduction trick for orbital-basis first-quantized simulation
