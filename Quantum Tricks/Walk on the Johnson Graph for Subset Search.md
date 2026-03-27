
> **Source:** Ambainis, arXiv:quant-ph/0311001 (2003); SIAM J. Comput. 37(1), 210–239 (2007)
> **Tags:** #trick #quantum-walk #element-distinctness #Johnson-graph #subset-search

## What it does

Solves problems of the form "find a $k$-element subset of $[N]$ satisfying property $P$" by doing a quantum walk on the Johnson graph $J(N, r)$ — the graph whose vertices are $r$-element subsets of $[N]$, with edges between subsets differing in exactly one element.

## The construction

1. Each vertex $S$ of $J(N, r)$ stores the data $\{(i, x_i) : i \in S\}$
2. A vertex is **marked** if the stored data contains a witness for property $P$ (e.g., a collision $x_i = x_j$ for element distinctness)
3. One walk step: swap element $i \in S$ for element $j \notin S$
   - **Cost: 1 query** (look up $x_j$ for the new element; $x_i$ is already known)
   - **Checking if marked: 0 queries** (check stored data internally)
4. Run [[Coined Quantum Walk Search on Graphs|coined walk search]] for $O(\sqrt{\binom{N}{r}/M})$ steps, where $M$ = number of marked vertices

## Element distinctness ($k = 2$)

Find $i \neq j$ with $x_i = x_j$ among $N$ items.

- Subsets of size $r$. A subset is marked if it contains a collision pair.
- Number of marked subsets: $\binom{N-2}{r-2} \cdot$ (number of collision pairs)
- Fraction marked: $\approx r^2/N^2$ (when there's one collision pair)
- Walk steps: $O(\sqrt{N^2/r^2} \cdot \sqrt{r}) = O(N/\sqrt{r})$... plus setup cost $O(r)$ for initial data loading
- Total: $O(r + N/\sqrt{r})$. Optimise: $r = N^{2/3}$, total = $O(N^{2/3})$

**Tight:** matches Shi's $\Omega(N^{2/3})$ lower bound.

## General $k$-element search

Find $k$ equal elements: use $r = N^{k/(k+1)}$, giving $O(N^{k/(k+1)})$.

Applications:
- **Triangle finding** ($n$-vertex graph): $O(n^{1.3})$ queries
- **$k$-clique finding:** via reduction to $k$-element distinctness

## When to reach for it

- Any problem where the answer is a small subset and you can check the property from stored data
- Element distinctness, collision problems without $r$-to-1 promise
- Graph property testing (triangles, cliques, subgraph detection)
- Problems where [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|BHT's $N^{1/3}$]] doesn't apply (no $r$-to-1 promise)

## Complexity

$O(N^{k/(k+1)})$ queries for $k$-element problems. Space: $O(N^{k/(k+1)})$ to store the subset data.

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
