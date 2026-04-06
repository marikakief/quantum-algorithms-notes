> **Source:** Shalev Ben-David, Andrew M. Childs, András Gilyén, William Kretschmer, Srijita Podder, Daochen Wang, *Symmetries, graph properties, and quantum speedups*, FOCS 2020; arXiv:2006.12760
> **Links:** [arXiv](https://arxiv.org/abs/2006.12760) · [FOCS](https://doi.org/10.1109/FOCS46700.2020.00066)
> **Tags:** #query-complexity #quantum-speedup #symmetry #graph-property #property-testing #well-shuffling #adjacency-list #adjacency-matrix #exponential-separation

---

## The computational problem

**When do symmetries prevent super-polynomial quantum speedups?** Given a partial Boolean function $f: P \to \{0,1\}$ with $P \subseteq \Sigma^n$ that is symmetric under a permutation group $G$ acting on $[n]$, what is the relationship between $R(f)$ (randomized query complexity) and $Q(f)$ (quantum query complexity)?

This addresses a foundational question: structure is necessary for exponential quantum speedups (total functions have at most polynomial separation by Beals et al.), but which symmetries are compatible with super-polynomial gaps?

---

## What the paper does

Three things:

1. **Adjacency matrix model:** Proves that any Boolean function symmetric under graph symmetries (or more generally, hypergraph symmetries) satisfies $R(f) = O(Q(f)^6)$. No exponential quantum speedup is possible for graph properties in the adjacency matrix model.

2. **Classification of symmetries:** Gives a complete dichotomy for primitive permutation groups. A collection of primitive groups $G$ allows super-polynomial quantum speedups if and only if $b(G) = n^{o(1)}$ (small minimal base size). Groups with $b(G) = n^{\Omega(1)}$ are "well-shuffling" and force $R(f) = Q(f)^{O(1)}$.

3. **Adjacency list model:** Constructs an explicit graph property testing problem in the adjacency list model (bounded-degree graphs) with an exponential quantum speedup — resolving open questions from [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes|Ambainis-Childs-Liu (2011)]] and Montanaro-de Wolf (2013).

The punchline-free summary: the answer to "can graph properties have exponential quantum speedups?" is model-dependent. No in the adjacency matrix model; yes in the adjacency list model.

---

## The algorithm / construction

### Part 1: Well-shuffling framework

**Definition (well-shuffling).** A collection of permutation groups $\mathcal{G}$ is *well-shuffling* if $\mathrm{cost}(G, r) = r^{\Omega(1)}$ for $G \in \mathcal{G}$, where $\mathrm{cost}(G, r)$ is the quantum query complexity of distinguishing a random permutation from $G$ from a random small-range function in $D_{n,r}$ (strings in $[n]^n$ using at most $r$ unique symbols).

**Key theorem (Theorem 23).** If $G$ is well-shuffling with power $a$, then every $f$ symmetric under $G$ satisfies $R(f) = O(Q(f)^a)$.

**Proof idea.** Take a $T$-query quantum algorithm $Q$ for $f$. Since $f$ is $G$-symmetric, $Q$ also works on $x \circ \pi$ for random $\pi \in G$. But $Q$ can't distinguish $\pi \in G$ from a small-range function $\alpha \in D_{n,r}$ when $r \approx T^a$. So $Q$ must also work on $x \circ \alpha$, which a classical algorithm can simulate by querying only the $\le r$ relevant bits of $x$. This gives $R(f) \le r = O(T^a)$.

### Part 2: Graph symmetries are well-shuffling

The authors show this via a chain of reductions:

1. The full symmetric group $S_n$ is well-shuffling (power 3), via the collision lower bound: distinguishing permutations from $r$-to-1 functions requires $\Omega(r^{1/3})$ quantum queries.

2. **Directed graphs:** $G_k = S_k^{\langle 2 \rangle}$ (action on ordered pairs), so $\mathrm{cost}(G_k, r) \ge \mathrm{cost}(S_k, \sqrt{r})/2 = \Omega(r^{1/6})$.

3. **Undirected graphs:** Reduce from directed via block system (pair $\{(x,y), (y,x)\}$ as one block). Cost preserved by Theorem 36.

4. **Hypergraphs:** $p$-uniform hypergraph symmetries are well-shuffling with power $3p$.

**Corollary (Theorem 2).** Any Boolean function symmetric under graph symmetries satisfies $R(f) = O(Q(f)^6)$.

### Part 3: Classification via CFSG

For primitive permutation groups, Liebeck's theorem (using the classification of finite simple groups) says: either $G$ is a subgroup of $S_\ell \wr S_m$ containing $A_m^{\times \ell}$ acting on $\binom{m}{p}^\ell$ points (case (i)), or $b(G) < 9\log_2 n$ (case (ii)).

Case (ii) groups have small base, so Proposition 44 constructs a super-polynomial speedup by embedding any hard problem (e.g., Simon's problem) into the $b(G)$ "free" coordinates.

Case (i) groups with $\ell p = O(1)$ are well-shuffling (proved by reduction through the chain: alternating group → power action → block system → merger).

**Dichotomy (Corollary 49):** For primitive $G$ on $[n]$:
- $b(G) = n^{\Omega(1)} \implies R(f) = Q(f)^{O(1)}$ for all $f$ symmetric under $G$
- $b(G) = n^{o(1)} \implies$ there exists $f$ symmetric under $G$ with $R(f) = Q(f)^{\omega(1)}$

### Part 4: Exponential speedup in the adjacency list model

Constructs a graph property $P_k$ based on "candy graphs" — multiple copies of [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|welded trees]], augmented with:
- **Antenna trees:** Binary trees attached to roots, providing advice edges
- **Advice edges:** Double edges connecting each body vertex to an antenna vertex, encoding whether the body vertex is in the weld (self-loop parity determines the classification)
- **Self-loops at roots:** Mark candies with even/odd parity, used to encode weld/interior distinction

**Quantum algorithm ($\mathrm{poly}(k)$ queries):**
1. Given any vertex $v$, determine if it's in the weld by:
   - Find the advice antenna vertex via the double edge
   - Navigate to the antenna root via Algorithm 2 (parent-finding by non-backtracking walks)
   - Use the [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs et al. quantum walk]] to find the paired root
   - Check self-loop parity of the root pair
2. With weld vertices marked, test the full structure classically with advice (binary tree test + weld test + completeness test + advice test)

**Classical lower bound ($2^{\Omega(k)}$ queries):**
- "No" instances use *self-welded* trees (leaves cycle within the same tree instead of between trees)
- Self-welded trees are $\Omega(1)$-far from the property because they're far from bipartite (Lemma 64)
- Classically, welded and self-welded trees are indistinguishable in $\mathrm{poly}(k)$ queries because the graph locally looks like an infinite binary tree to a random walker (extends the lower bound of Childs et al.)
- A unified simulator $G_s$ produces identical responses to both $G_1$ (yes instances) and $G_2$ (no instances) for sub-exponentially many queries

---

## Key results

| Result | Statement |
|---|---|
| Graph properties, adjacency matrix | $R(f) = O(Q(f)^6)$ — at most polynomial separation |
| $p$-uniform hypergraph properties | $R(f) = O(Q(f)^{3p})$ |
| Primitive group classification | $b(G) = n^{\Omega(1)} \iff$ well-shuffling $\iff$ no super-polynomial speedup |
| Adjacency list property testing | $Q(P_k) = \mathrm{poly}(k)$, $R(P_k) = 2^{\Omega(k)}$ — exponential gap |

---

## Comparison with prior work

| Paper | Result | This paper extends/resolves |
|---|---|---|
| Aaronson-Ambainis (2014) | Fully symmetric functions: no exponential speedup | Generalised to graph/hypergraph symmetries |
| Chailloux (2018) | $R(f) = O(Q(f)^3)$ for $S_n$-symmetric $f$ | Generalised to all well-shuffling groups |
| [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes\|Ambainis-Childs-Liu (2011)]] | $\tilde{O}(N^{1/3})$ quantum property testing; asked if exponential speedup possible | Resolved: exponential speedup exists in adjacency list model |
| [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes\|Childs et al. (2003)]] | Exponential speedup for welded trees traversal | Lifted to a graph *property testing* separation |

---

## Limits / caveats

- The $R(f) = O(Q(f)^6)$ bound for graph properties in the adjacency matrix model is unlikely tight. The best known lower bound on the exponent is 2 (from OR), and the authors conjecture it might be closer to 3 (the $S_n$ case).
- The classification uses the classification of finite simple groups (via Liebeck's theorem). Whether the dichotomy can be proved without CFSG remains open.
- The adjacency list exponential separation is for a somewhat artificial property $P_k$ (candy graphs with advice edges). Whether "natural" graph properties like bipartiteness can exhibit exponential quantum speedup in the adjacency list model is open.
- The imprimitive group case isn't fully resolved: Conjecture 72 posits that well-shuffling is exactly characterised by conditions (i)–(iv) of Corollary 55.

---

## Reusable ideas

1. **[[Well-Shuffling Reduction for Quantum Speedup Bounds]]** — The technique of reducing "does this symmetry group allow super-polynomial speedups?" to "can you distinguish random permutations from small-range functions?" is clean and general. The chain of cost-preserving transformations (power action, block systems, mergers) provides a toolkit for new symmetry groups.

2. **[[Advice Edge Encoding for Quantum Walk Property Testing]]** — The construction that encodes weld/interior information via double edges connecting to antenna trees with self-loop parity. Solves the chicken-and-egg problem: you need to mark weld vertices to test the structure, but marking weld vertices is exactly what a quantum computer can do (via the welded trees walk) and a classical computer cannot.

3. **[[Embedding Hard Problems into Free Base Coordinates]]** — For a group $G$ with small base $b(G)$, any hard promise problem can be made $G$-symmetric by encoding the original input in the $b(G)$ base coordinates. Since $b(G)$ coordinates determine $\pi \in G$, a quantum algorithm queries them first, recovers $\pi$, and then solves the original problem. This is the generic construction for super-polynomial speedups under small symmetry groups.

---

## References within this paper

- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs et al. (2003)]] — welded trees exponential separation; basis for the adjacency list construction
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes|Ambainis-Childs-Liu (2011)]] — quantum graph property testing, open question resolved here
- Aaronson-Ambainis (2014) — symmetric functions don't have exponential speedups; predecessor to the well-shuffling framework
- Chailloux (2018) — $R(f) = O(Q(f)^3)$ for fully symmetric functions
- Beals-Buhrman-Cleve-Mosca-de Wolf (2001) — total functions have polynomial quantum-classical relationship
- Liebeck (1984) — classification of minimal base sizes of primitive groups via CFSG
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — block-encoding framework, referenced for implementing adjacency list oracles
- Zhan-Kimmel-Hassidim (2012) — 1-Fault Direct Trees problem, used for deep wreath product speedups
- Goldreich-Ron (1997) — classical property testing in adjacency list model

---

## Cross-links

### Paper notes
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — welded trees that form the core of the adjacency list separation
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]] — precursor showing ballistic vs. diffusive transport on simpler glued trees
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]] — the quantum property testing paper whose open question this resolves
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — walk framework used in the quantum algorithm
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous-time walk search, related quantum walk algorithmic technique
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]] — adversary methods for query complexity lower bounds

### Trick cards
- [[Well-Shuffling Reduction for Quantum Speedup Bounds]] — new (this batch)
- [[Advice Edge Encoding for Quantum Walk Property Testing]] — new (this batch)
- [[Embedding Hard Problems into Free Base Coordinates]] — new (this batch)
- [[Symmetry Reduction to Column Subspace in Quantum Walks]] — used in the welded trees subroutine
- [[Coined Quantum Walk Search on Graphs]] — related walk search techniques
