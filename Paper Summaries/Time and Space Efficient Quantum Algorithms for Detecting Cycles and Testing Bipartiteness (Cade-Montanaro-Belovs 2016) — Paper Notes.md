> **Source:** Chris Cade, Ashley Montanaro, and Aleksandrs Belovs, *Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness*, arXiv:1610.00581 (2016)
> **Links:** [arXiv](https://arxiv.org/abs/1610.00581) · [PDF](https://arxiv.org/pdf/1610.00581)
> **Zoo:** #317 in [[Quantum algorithm zoo]]
> **Tags:** #paper #quantum-algorithms #graph-property-testing #span-programs #quantum-walks #space-efficient #query-complexity

# Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade--Montanaro--Belovs 2016) — Paper Notes

## One-line takeaway

They get the optimal $\widetilde O(n^{3/2})$ matrix-model quantum time for cycle detection and bipartiteness while using only $O(\log n)$ space, by reducing cycles to bounded-length $s$-$t$ connectivity in a small implicit covering graph.

## Problem and input models

Input: an undirected graph $G=(V,E)$ with $|V|=n$.

Two access models are treated.

1. **Adjacency matrix model.** Query bit $A_{uv}\in\{0,1\}$ tells whether $(u,v)\in E$. The algorithm counts time, not only queries; the oracle is not charged against the working-space bound.
2. **Adjacency array model.** Degrees $d_1,\ldots,d_n$ are given for free, and each vertex $u$ has an injective neighbour array
   $$
   f_u:[d_u]\to[n]
   $$
   returning the $j$th neighbour of $u$. The graph is undirected and not a multigraph. Let $m=|E|$ and $d_m=\max_u d_u$.

Tasks:

- **Cycle detection / forest testing:** decide whether $G$ contains any cycle, and in the positive case find a vertex on a cycle.
- **Bipartiteness testing:** decide whether $G$ contains an odd cycle.

The paper uses $O(\log n)$ classical and quantum bits/qubits of working storage. It assumes QRAM-like coherent oracle access for graph queries; the oracle itself is external to the space count.

## Main results

### Adjacency matrix model

There is a bounded-error quantum algorithm for cycle detection in time
$$
\widetilde O(n^{3/2})
$$
using $O(\log n)$ space.

A modification decides bipartiteness in the same time and space.

This matches the known $\Omega(n^{3/2})$ quantum query lower bounds for cycle detection and bipartiteness up to polylog factors, while avoiding the $O(n\log n)$ coherent storage used by reconstruction-style algorithms.

### Adjacency array model

There are $O(\log n)$-space algorithms for cycle detection and bipartiteness running in time
$$
\widetilde O(n\sqrt{d_m}).
$$

The paper also proves $\Omega(n)$ quantum query lower bounds in this model by a parity reduction. The query complexity can be $\widetilde O(n)$ with other methods, but those use $O(n\log n)$ space; this paper trades time for logarithmic space.

## Reduction: cycles to $s$-$t$ connectivity

Fix an arbitrary orientation of each edge of $G$, e.g. orient $(u,v)$ from the smaller label to the larger label. For a chosen vertex $k\in V$, build an implicit graph
$$
H=(V',E')
$$
with
$$
V'=\{s,t\}\cup\{v_b:v\in V,\ b\in\{0,1,2\}\}.
$$
For each directed edge $(u,v)$ of $G$ and each $b\in\{0,1,2\}$, put an edge
$$
(u_b,v_{b+1\bmod 3})
$$
in $H$. Add $(s,k_0)$ and $(t,k_1)$.

Interpret the second register $b$ as a mod-3 charge. Traversing an oriented edge changes $b$ by $+1$ or $-1$ depending on whether the traversal agrees with the orientation.

For a cycle $C$, let
$$
D=p-q
$$
where $p$ is the number of clockwise edges and $q$ the number of anticlockwise edges. If $D\not\equiv0\pmod3$ and $k\in C$, then $s$ and $t$ are connected in $H$ by a path of length at most $2|C|+2$.

If $G$ has no cycle, then $s$ and $t$ are disconnected in $H$.

The only issue is a cycle whose orientation imbalance is $0\bmod3$.

## Random orientation repair

To avoid the mod-3 failure case, colour vertices using a pairwise-independent hash function
$$
h:[n]\to\{0,1\}
$$
requiring $O(\log n)$ bits. Flip the orientations of edges incident to $k$ whose other endpoint has colour 1.

If $k$ lies on a cycle, the two cycle neighbours of $k$ get different colours with probability $1/2$. In that case exactly one incident cycle edge flips, changing $D\bmod3$ and making the reduction succeed. If the cycle was already good, the colouring can spoil it with probability at most $1/4$, so the success probability remains constant.

This is the neat technical move: only pairwise independence is needed, so the randomisation stays logarithmic-space.

## Local test for “is $k$ on a short cycle?”

Given $k$ and a length guess $d$, run the Belovs--Reichardt span-program algorithm for $s$-$t$ connectivity on the implicit $H$.

Their theorem: if an $N$-vertex graph has an $s$-$t$ path of length at most $d$, $s$-$t$ connectivity can be decided in
$$
\widetilde O(N\sqrt d)
$$
time and $O(\log N)$ space.

Here $N=O(n)$ and the path length in $H$ is $O(d)$ when $k$ is on a cycle of length at most $d$. So the subroutine costs
$$
\widetilde O(n\sqrt d)
$$
time and $O(\log n)$ space.

It accepts with constant probability if $k$ lies on a cycle of length $\le d$, and rejects with high probability if $G$ is acyclic.

The graph $H$ is never materialised. A query to adjacency in $H$ is computed by checking the relevant edge of $G$, the vertex labels, the mod-3 registers, and the hash-colouring orientation flip.

## Global cycle detection

Use Boyer--Brassard--Høyer--Tapp QSearch over vertices, with increasing guesses
$$
d=2^i,
\qquad i=1,\ldots,\lceil\log_2 n\rceil.
$$

For a cycle of length $\ell$, once $d\ge\ell$, there are $\ell$ good vertices. QSearch needs expected
$$
O(\sqrt{n/\ell})
$$
Grover iterations. Each Grover oracle call costs $\widetilde O(n\sqrt d)$, and for the first good scale $d\le2\ell$, the product is
$$
\widetilde O(\sqrt{n/\ell}\cdot n\sqrt\ell)=\widetilde O(n^{3/2}).
$$

The logarithmic scan over length guesses keeps the total at $\widetilde O(n^{3/2})$.

## Bipartiteness

Replace the mod-3 lift by a mod-2 lift. Split each vertex into two copies rather than three.

In the mod-2 lift, even cycles do not connect the two copies in the relevant way, while odd cycles do. Since a graph is bipartite iff it has no odd cycle, the same framework gives bipartiteness testing in
$$
\widetilde O(n^{3/2})
$$
time and $O(\log n)$ space, without the random colouring step.

## Adjacency array model

In the array model, if $m\ge n$ the graph has a cycle and can be rejected/accepted accordingly. For $m<n$, the paper uses Belovs' electric-network quantum walk for $s$-$t$ connectivity on the implicit lift graph.

Given a vertex $u_b$ in the lift graph, its neighbours can be computed from the neighbour array of $u$ in $G$ plus the mod register update. The local reflection needs a uniform state over neighbours:
$$
|\phi_u\rangle=\frac1{\sqrt{d_u}}\sum_{i\in[d_u]} |f_u(i)\rangle.
$$

Preparing this in superposition over $u$ costs $O(\sqrt{d_m})$ queries by exact amplitude amplification. Belovs' walk needs $\widetilde O(n)$ applications, giving
$$
\widetilde O(n\sqrt{d_m})
$$
time.

The bound is worse than the best query bound in high-degree graphs, but the point is space: the algorithm stores only $O(\log n)$ qubits/bits.

## Lower bounds

For the adjacency matrix model, the paper relies on known $\Omega(n^{3/2})$ quantum query lower bounds for bipartiteness and cycle detection.

For the adjacency array model, it gives a parity reduction. Build a two-level graph from a bit string $x\in\{0,1\}^p$ where traversing column $i$ swaps levels iff $x_i=1$. Depending on parity, the graph has one or two cycles, or contains an odd cycle after a dummy-bit tweak. Thus deciding cycle detection or bipartiteness would decide parity, giving
$$
\Omega(n)
$$
queries.

## Technical assessment

The result is not a new query-complexity exponent. The value is the time-and-space implementation: it shows that optimal $\widetilde O(n^{3/2})$ graph-property algorithms need not keep a reconstructed sparse graph in coherent memory.

The reusable idea is the lift graph. It packages a cycle witness into an $s$-$t$ path whose length is controlled by the cycle length, and it can be queried locally from the original graph. That is exactly the kind of trick worth preserving in the vault.

The array-model section is more of a trade-off: logarithmic space costs a $\sqrt{d_m}$ factor. Still useful for implicit massive graphs where $O(n\log n)$ storage is the bottleneck.

## Tricks extracted

1. [[Mod-k Lift from Cycle Detection to st-Connectivity]] — encode oriented cycle imbalance as a path between copies.
2. [[Pairwise-Independent Edge-Flipping to Repair Modular Cycle Tests]] — fix bad mod-3 cycles using only $O(\log n)$ random bits.
3. [[Length-Doubling QSearch for Unknown Cycle Size]] — combine local short-cycle tests with unknown-solution Grover search.
4. [[Log-Space Span-Program st-Connectivity as a Subroutine]] — use Belovs--Reichardt connectivity without storing the graph.
5. [[Adjacency-Array Quantum Walk via Neighbour-State Amplification]] — implement walk reflections by preparing neighbour superpositions with exact amplitude amplification.
6. [[Parity Gadget Lower Bound for Array-Model Graph Properties]] — encode parity in one/two-cycle structure.

## Related notes

- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]] — earlier graph property testing with element distinctness-style collision search and bounded-degree promises.
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — quantum walk search machinery behind marked-vertex detection.
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] — related use of Grover search as a wrapper around a structured predicate.
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] — context for hidden subgroup/graph-property reductions, not a direct ancestor.
- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]] — another Montanaro paper where the key point is not just query count but implementing statistics under a constrained input model.
