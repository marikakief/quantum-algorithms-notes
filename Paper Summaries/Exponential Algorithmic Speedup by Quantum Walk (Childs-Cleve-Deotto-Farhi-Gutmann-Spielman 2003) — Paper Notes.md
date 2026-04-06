> **Source:** Andrew M. Childs, Richard Cleve, Enrico Deotto, Edward Farhi, Sam Gutmann, Daniel A. Spielman, *Exponential algorithmic speedup by quantum walk*, arXiv:quant-ph/0209131, Proc. 35th ACM STOC, 59–68 (2003)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0209131) · [STOC](https://doi.org/10.1145/780542.780552)
> **Tags:** #quantum-walk #oracle-separation #exponential-speedup #graph-traversal #foundational

---

## What the paper does

Constructs a black-box graph traversal problem with an exponential quantum-classical separation, where the quantum algorithm uses a **continuous-time quantum walk** — not a Fourier transform. This is the first exponential speedup from a technique other than the QFT family, and it established quantum walks as a genuinely different algorithmic primitive.

The problem: given an oracle for a graph (vertex names and their neighbours), find the name of a designated EXIT vertex starting from a designated ENTRANCE. The quantum walk traverses the graph in $\mathrm{poly}(n)$ time; any classical algorithm needs $2^{\Omega(n)}$ queries.

---

## The graph

Start with two balanced binary trees of height $n$, joined at the leaves. The original version $G_n$ (from [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes|Childs, Farhi & Gutmann 2002]]) connects leaves by a simple identification — but this can be classically traversed in $O(n^2)$ time by backtracking from the central column (you can detect the column by vertex degree).

The fix: $G'_n$ connects the $2^n$ left-tree leaves to the $2^n$ right-tree leaves via a **random alternating cycle** instead of a simple identification. You can no longer detect which way is "forward" once you reach the leaves.

Vertex names are random $2n$-bit strings (exponentially more names than vertices), so you can't just guess the EXIT name. The oracle takes a vertex name and returns its neighbours' names.

---

## The quantum algorithm

### Continuous-time quantum walk

The Hamiltonian is $\gamma$ times the adjacency matrix of $G'_n$:

$$\langle a | H | a' \rangle = \begin{cases} \gamma & \text{if } aa' \in G \\ 0 & \text{otherwise} \end{cases}$$

This is the natural quantum analogue of a continuous-time classical random walk (replace the generator matrix with the adjacency matrix, Markov dynamics with Schrödinger dynamics).

### Column-subspace reduction

The structure of $G'_n$ is highly symmetric *within* each column. Define column states:

$$|\text{col } j\rangle = \frac{1}{\sqrt{N_j}} \sum_{a \in \text{column } j} |a\rangle$$

Despite the random leaf connections, the column subspace is **invariant under $H$** — every vertex in column $j$ connects to the same number of vertices in columns $j \pm 1$. So the quantum walk starting from ENTRANCE (= $|\text{col } 0\rangle$) reduces to a walk on a line of $2n+2$ vertices.

The line has uniform hopping rate 1 everywhere except a $\sqrt{2}$ defect at the centre (where the trees meet). Setting $\gamma = 1/\sqrt{2}$, the effective 1D Hamiltonian is:

$$\langle \text{col } j | H | \text{col}(j+1) \rangle = \begin{cases} 1 & j \neq n \\ \sqrt{2} & j = n \end{cases}$$

### Wave-packet propagation

On an infinite uniform line, the propagator is a Bessel function: $G(j, k, t) = (-i)^{k-j} J_{k-j}(2t)$. This propagates as left- and right-moving wave packets at speed 2.

The $\sqrt{2}$ defect has transmission coefficient:

$$|T(p)|^2 = \frac{8 \sin^2 p}{1 + 8 \sin^2 p}$$

which is close to 1 for most momenta — the wave packet mostly passes through the centre.

Boundaries cause reflections but don't impede propagation (shown via the method of images, Eq. 40 in the paper).

### Hitting time bound

**Theorem 3:** Run the quantum walk for time $t$ chosen uniformly in $[0, n^4/(2\varepsilon)]$, then measure. The probability of finding EXIT is $> \frac{1}{2n}(1 - \varepsilon)$.

The proof has two parts:
1. **Lemma 1:** The time-averaged probability of finding EXIT is $\geq \frac{1}{2n} - \frac{2}{\tau \Delta E}$, where $\Delta E$ is the minimum eigenvalue gap
2. **Lemma 2:** $\Delta E > \frac{2\pi^2}{(1+\sqrt{2})n^3} + O(1/n^4)$, from the quantization condition $\frac{\sin((n+1)p)}{\sin np} = \pm \sqrt{2}$

Repeat $O(n)$ times → high probability of success. Total: $\mathrm{poly}(n)$ oracle queries.

---

## Implementing the quantum walk on a quantum computer

This is the technically interesting part — the quantum walk Hamiltonian $H$ is defined by the graph, which is given as an oracle, so $H$ isn't obviously local. The implementation uses five [[Hamiltonian simulation]] tools:

1. **Local terms** — simulate any $O(1)$-qubit Hamiltonian directly
2. **[[Product-Formula Time-Slicing for Local Hamiltonians|Lie product formula]]** — simulate $H_1 + \cdots + H_k$ from simulations of each $H_i$
3. **Commutation** — simulate $i[H_1, H_2]$ from simulations of $H_1, H_2$ (not needed here, but listed)
4. **[[Unitary Conjugation for Hamiltonian Simulation|Unitary conjugation]]** — simulate $UHU^\dagger$ from simulation of $H$ and implementation of $U$
5. **Tensor product** — simulate $\bigotimes_j H_j$ when eigenvalue products are efficiently computable

The construction:
- Define a swap operator $T$ that exchanges the first and second name registers (conditioned on a flag bit): $T|a, b, 0\rangle = |b, a, 0\rangle$
- Decompose $T = (\bigotimes_l S^{(l, 2n+l)}) \otimes |0\rangle\langle 0|$ where $S$ swaps two qubits — this is a tensor product of local terms
- The graph oracle $V_c$ maps $|a, 0, 0\rangle \to |a, v_c(a), f_c(a)\rangle$ where $v_c(a)$ is the neighbour of $a$ along colour $c$
- The walk Hamiltonian is $H = \sum_c V_c^\dagger T V_c$ — a sum of unitarily conjugated copies of $T$

This requires a consistent **edge colouring** of the graph (using $k = O(1)$ colours), provided by the oracle. The colouring is shown to be classically useless (a classical algorithm can make up its own colouring as it goes).

---

## The classical lower bound

**Theorem 9:** Any classical algorithm making $\leq 2^{n/6}$ oracle queries finds EXIT with probability $\leq 4 \cdot 2^{-n/6}$.

The proof proceeds through a sequence of game reductions:
- **Game 1 → Game 2:** Can't guess random vertex names (exponentially many possible names)
- **Game 2 → Game 3:** Edge colouring is useless (can make it up yourself)
- **Game 3 → Game 4:** Winning if you find a cycle is strictly easier
- **Game 4 → Game 5:** Connected subgraph traversal is equivalent to random embedding of a binary tree
- **Game 5 bound:** A tree with $\leq 2^{n/6}$ vertices almost never finds EXIT or a cycle in $G'_n$ — the random leaf cycle makes it exponentially unlikely that two explored paths meet in the same subtree

The core combinatorial argument: the right half of $G'_n$ decomposes into $2^{n/2}$ subtrees of height $n/2$. A classical traversal tree of size $t$ reaches column $n + n/2$ with probability $\leq t^2 \cdot 2^{1-n/2}$ (you'd need to go right $n/2$ times in a row). And finding a cycle requires two paths to enter the same subtree via the random cycle, probability $\leq t^2 \cdot 2^{-n/2}$.

---

## Key results

| Result | Statement |
|---|---|
| Quantum upper bound | EXIT found in $\mathrm{poly}(n)$ oracle queries with high probability |
| Classical lower bound | Any classical algorithm needs $2^{\Omega(n)}$ queries |
| Separation | Exponential (query complexity) |
| Oracle type | BQP $\neq$ BPP relative to this oracle |
| Walk implementation | $\mathrm{poly}(n)$ gates per walk step using edge-coloured oracle |

---

## Limits / caveats

- **Oracular separation only.** This doesn't prove BQP $\neq$ BPP unconditionally — it constructs an oracle relative to which they differ. But it does show quantum walks are a source of exponential speedup.
- **The algorithm finds EXIT but not a path.** It teleports to the exit via the walk dynamics; it doesn't construct an explicit path through the graph.
- **Edge colouring required** for the general implementation. For bipartite graphs starting from a known vertex, Appendix B shows how to construct the colouring from the oracle's output ordering — but the general case (arbitrary graph, arbitrary initial state) remains open.
- **The graph family $G'_n$ is artificial** — it's designed for the separation. Whether continuous-time quantum walks give exponential speedup for natural computational problems was left open. (Childs later used similar ideas for the universal quantum walk algorithm.)

---

## Reusable ideas

1. [[Column-Subspace Reduction for Symmetric Graphs]] — when a graph has a symmetry that makes each "column" of vertices equivalent, the quantum walk reduces to a walk on a much smaller line. The random structure doesn't break this symmetry as long as it's "balanced" within columns.
2. [[Oracle-to-Hamiltonian Simulation via Swap Conjugation]] — simulate a graph-adjacency Hamiltonian from an oracle by conjugating a swap operator with oracle queries: $H = \sum_c V_c^\dagger T V_c$. Turns an oracle problem into [[Hamiltonian simulation]].

---

## References within this paper

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — first quantum speedup (1-query advantage)
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — exponential separation via QFT
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring; all prior exponential speedups used QFT
- Farhi & Gutmann (1998, PRA 58, 915) — introduced continuous-time quantum walks
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes|Childs, Farhi & Gutmann (2002)]] — exponentially faster hitting time for quantum vs classical walk on $G_n$ (but not an algorithmic separation)
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — [[Hamiltonian simulation]] tools used in the implementation (ref [26])
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — discrete-time walk framework (appeared shortly after)
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — later unified continuous and discrete walks

---

## Cross-links

### Paper notes
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — establishes equivalence of continuous and discrete quantum walks; builds on this paper
- [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]] — takes continuous-time walk from an algorithmic primitive to a universal computational model via scattering on graph widgets
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous-time walk for spatial search (same technique, different problem)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — discrete-time walk framework with quadratic speedup
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — algorithmic application of discrete quantum walks
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — black-box [[Hamiltonian simulation]] extending the oracle-to-Hamiltonian ideas here
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — product-formula simulation tools used in the implementation
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — the classical lower bound in that paper directly inherits the exponential separation from this glued-trees result; the oscillator problem is BQP-complete via a reduction through this paper

### Trick cards
- [[Column-Subspace Reduction for Symmetric Graphs]]
- [[Oracle-to-Hamiltonian Simulation via Swap Conjugation]]
- [[Continuous-Time Quantum Walk Search]] — related continuous-time walk technique
- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]] — later development of the oracle-to-walk idea
- [[Product-Formula Time-Slicing for Local Hamiltonians]] — used in the Lie [[Product Formulas]] step
