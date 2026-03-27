
> **Source:** Childs, Cleve, Deotto, Farhi, Gutmann, Spielman, arXiv:quant-ph/0209131 (2003)
> **Tags:** #trick #quantum-walk #hamiltonian-simulation #oracle

## What it does
Simulates a graph-adjacency Hamiltonian on a quantum computer when the graph is specified by a black-box oracle, by conjugating a swap operator with oracle queries.

## The trick

The Hamiltonian for a quantum walk on graph $G$ is the adjacency matrix: $\langle a | H | a' \rangle = \gamma$ if $a, a'$ are connected. This is highly nonlocal (it acts on vertex names that are $2n$-bit strings), so it can't be simulated directly as a sum of local terms.

The construction uses three registers: vertex name $a$ ($2n$ bits), auxiliary name $b$ ($2n$ bits), flag $r$ (1 bit).

**Step 1:** Define a swap operator $T|a, b, 0\rangle = |b, a, 0\rangle$, $T|a, b, 1\rangle = 0$. This decomposes as $T = (\bigotimes_l S^{(l, 2n+l)}) \otimes |0\rangle\langle 0|$ — a tensor product of 2-qubit swaps, hence efficiently simulable.

**Step 2:** Given an edge-coloured oracle $V_c$ that maps $|a, 0, 0\rangle \to |a, v_c(a), f_c(a)\rangle$ (where $v_c(a)$ is the neighbour along colour $c$, and $f_c$ flags whether the edge exists), construct:

$$H = \sum_c V_c^\dagger \, T \, V_c$$

**Why it works:** $H|a, 0, 0\rangle = \sum_{c: v_c(a) \in G} |v_c(a), 0, 0\rangle$ — exactly the adjacency matrix action, restricted to the "clean" subspace $|a, 0, 0\rangle$.

Simulate $e^{-iHt}$ via [[Product-Formula Time-Slicing for Local Hamiltonians|Lie product formula]]: each term $V_c^\dagger T V_c$ is simulated by unitary conjugation (simulate $T$, sandwich with oracle calls $V_c$). The number of colours $k$ is $O(1)$ for bounded-degree graphs.

## When to reach for it
- Any oracle problem where you need to simulate a quantum walk on a graph given by a black box
- Converting graph oracle access into [[Hamiltonian simulation]]
- When the "natural" Hamiltonian is nonlocal but structured as an adjacency matrix

## Complexity
- $O(k)$ oracle calls per Trotter step (one per colour)
- $O(n)$ gates to simulate $T$ (just 2-qubit swaps)
- $\mathrm{poly}(n)$ total for the full walk simulation to time $t = \mathrm{poly}(n)$

## Caveat
Requires a consistent edge colouring of the graph — no two edges incident on the same vertex share a colour. For bipartite graphs starting from a known vertex, the colouring can be constructed from the oracle's output ordering (Appendix B of the paper). For general graphs and general initial states, whether colouring is necessary remains open.

## Related notes
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]] — later development of oracle-to-walk ideas
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — the simulation toolkit used here
