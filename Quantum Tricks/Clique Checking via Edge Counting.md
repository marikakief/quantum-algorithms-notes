
> **Source:** Berry, Su, Gyurik, King, Basso, Del Toro Barba, Rajput, Wiebe, Dunjko, Babbush, arXiv:2209.13581, PRX Quantum **5**, 010319 (2024)
> **Tags:** #trick #graph-algorithm #clique #fault-tolerant #oracle-construction

## What it does

Given an $n$-qubit computational basis state $|x\rangle$ of Hamming weight $k$ (representing a subset of $k$ vertices), determines whether $x$ is a $k$-clique in a graph $G$ using $3|E| + 2\log k$ Toffolis (edge database) or $2|E^C|$ Toffolis (missing-edge database), where $|E|$ and $|E^C|$ are the numbers of edges and missing edges respectively.

## The trick

**Edge database approach:**

1. For each edge $(u,v)$ in the classical edge list, apply a Toffoli with qubits $u$ and $v$ as controls and an ancilla as target. If both qubits are $|1\rangle$ (both vertices selected), the ancilla flips. Cost: $|E|$ Toffolis.
2. Sum all the ancilla bits using the efficient bit-summing procedure from [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders et al. 2020]]. Cost: $\leq 2|E|$ Toffolis, $O(\log|E|)$ ancillas.
3. Check if the sum equals $\binom{k}{2}$ (the number of edges in a complete subgraph on $k$ vertices). Cost: $2\log k$ Toffolis.

Total: $3|E| + 2\log k$ Toffolis. For reflection (needed in amplitude amplification): $6|E| + 2\log k$.

**Missing-edge database approach:**

When $|E| > n^2/5$, it's cheaper to check for *absence* of missing edges. For each missing edge, check if both endpoints are selected. If any check succeeds, the state is not a clique.

1. Group the $|E^C|$ missing edges into $\sim\sqrt{|E^C|}$ groups.
2. Within each group, apply Toffolis and combine with a multiply-controlled Toffoli (OR). Cost: $\sqrt{|E^C|} - 1$ per group.
3. Combine group results with another multiply-controlled Toffoli. Total: $2|E^C| - 1$ Toffolis.

## When to reach for it

- Quantum algorithms on clique complexes (TDA, homology computation) where you need to project onto or amplify the clique subspace.
- Any setting where a classical edge/missing-edge list is available and you need a coherent clique oracle.
- Switchover point: use edge database for $|E| < n^2/5$, missing-edge database otherwise.

## Complexity

| Method | Toffolis (check) | Toffolis (reflect) | Ancillas |
|---|---|---|---|
| Edge database | $3\|E\| + 2\log k$ | $6\|E\| + 2\log k$ | $O(\log\|E\|)$ |
| Missing-edge database | $2\|E^C\|$ | $4\|E^C\|$ | $O(\sqrt{\|E^C\|})$ |

## Caveat

Assumes the graph is given as a classical list of edges (or missing edges), not via an oracle. This is different from the oracle model used by Lloyd et al. (2016) and Gunn & Kornerup (2019), where the graph was accessed through distance queries at $O(k^2)$ oracle calls. The two models aren't directly comparable, but for any concrete implementation you'd need an explicit edge list anyway.

The ancilla cost of the missing-edge approach ($O(\sqrt{|E^C|})$) could be significant for very dense graphs. For the $K(16,16)$ example in the paper, $|E^C| = 1920$, so $\sqrt{|E^C|} \approx 44$ ancillas — easily affordable.

## Related notes
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]]
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Sum of Tree Sums for Bit Counting]]
