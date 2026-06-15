
> **Source:** Ambainis, arXiv:quant-ph/0311001 (2003); SIAM J. Comput. 37(1), 210–239 (2007)
> **Tags:** #trick #quantum-walk #element-distinctness #Johnson-graph #subset-search

## What it does

Solves some subset-search problems by doing a quantum walk on the Johnson graph $J(N, r)$ — the graph whose vertices are $r$-element subsets of $[N]$, with edges between subsets differing in exactly one element. The useful cases are those where stored subset data makes checking cheap and adjacent updates much cheaper than rebuilding the subset.

## The construction

1. Each vertex $S$ of $J(N, r)$ stores the data $\{(i, x_i) : i \in S\}$
2. A vertex is **marked** if the stored data contains a witness for property $P$ (e.g., a collision $x_i = x_j$ for element distinctness)
3. One walk step: swap element $i \in S$ for element $j \notin S$
   - **Cost: constant queries** in the reversible query model (load the new value, and often unquery the removed value depending on the implementation)
   - **Checking if marked: 0 queries** (check stored data internally)
4. Analyze the marked fraction and the Johnson-graph spectral gap. In MNRS notation, the walk-search cost is governed by

$$
S + \frac{1}{\sqrt{\epsilon}}\left(\frac{1}{\sqrt{\delta}}U + C\right),
$$

where $\epsilon$ is the marked stationary probability, $\delta=\Theta(1/r)$ for $J(N,r)$, $S$ is setup, $U$ is update, and $C$ is checking.

## Element distinctness ($k = 2$)

Find $i \neq j$ with $x_i = x_j$ among $N$ items.

- Subsets of size $r$. A subset is marked if it contains a collision pair.
- Number of marked subsets: $\binom{N-2}{r-2} \cdot$ (number of collision pairs)
- Fraction marked: $\approx r^2/N^2$ (when there's one collision pair)
- Walk steps: $O(\sqrt{N^2/r^2} \cdot \sqrt{r}) = O(N/\sqrt{r})$... plus setup cost $O(r)$ for initial data loading
- Total: $O(r + N/\sqrt{r})$. Optimise: $r = N^{2/3}$, total = $O(N^{2/3})$

**Tight:** matches Shi's $\Omega(N^{2/3})$ lower bound.

## General $k$-element search

Find $k$ equal elements in Ambainis's framework: use $r = N^{k/(k+1)}$, giving $O(N^{k/(k+1)})$.

Applications:
- **Triangle finding** ($n$-vertex graph): $O(n^{1.3})$ queries
- **Fixed subgraph/clique finding:** only after specifying the data structure and checking subroutine; update/check costs can dominate

## When to reach for it

- Subset-search problems where a marked subset can be recognized from stored data and one-element updates are cheap
- Element distinctness, collision problems without $r$-to-1 promise
- Graph-property algorithms such as triangle or fixed-subgraph finding, once their checking subroutines are accounted for
- Problems where [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|BHT's $N^{1/3}$]] doesn't apply (no $r$-to-1 promise)

## Complexity

$O(N^{k/(k+1)})$ queries for Ambainis-style $k$-distinctness. This is not a universal bound for every $k$-element certificate problem. Space is $O(N^{k/(k+1)})$ to store the subset data in the basic implementation.

## Caveat

The Johnson graph has $\binom{N}{r}$ vertices — exponentially many. The walk never explicitly constructs this graph; it only maintains one vertex (subset) at a time and walks locally. But the analysis uses the full graph structure.

Setup cost matters: loading the initial $r$ elements costs $r$ queries, which dominates when $r$ is large. The balance between setup and walk costs determines the optimal $r$.

## Related notes

- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — the full paper
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] — $O(N^{1/3})$ with $r$-to-1 promise
- [[Classical Preprocessing plus Grover Search]] — BHT's hybrid approach
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
