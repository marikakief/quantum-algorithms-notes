> **Source:** Dorit Aharonov, Itai Arad, Elad Eban, and Zeph Landau, *Polynomial Quantum Algorithms for Additive Approximations of the Potts Model and Other Points of the Tutte Plane*, arXiv:quant-ph/0702008 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0702008)
> **Tags:** #Tutte-polynomial #Potts-model #BQP-complete #Temperley-Lieb #non-unitary #combinatorics #partition-function

---

## The computational problem

**Tutte polynomial approximation:** Given a planar graph $G = (V, E)$ with complex edge weights $\{v_e\}$ and a complex parameter $q$, approximate the multivariate Tutte polynomial

$$Z_G(q; v) = \sum_{A \subseteq E} q^{k(A)} \prod_{e \in A} v_e$$

where $k(A)$ is the number of connected components of the subgraph $(V, A)$.

This polynomial encodes an enormous range of combinatorial and physical quantities:
- The **Potts model partition function** at any temperature and any number of colours (via $v_e = e^{\beta J_e} - 1$)
- The **Jones polynomial** of alternating links (at roots of unity)
- The **chromatic polynomial** (number of proper $q$-colourings, via $T \to 0$ limit in anti-ferromagnetic case)
- The number of **spanning trees** (at $q = 0, v = -1$)
- **Network reliability** (at $q = 1$)
- The standard Tutte polynomial $T_G(x,y)$ via the change of variables $q = (x-1)(y-1)$, $v = y - 1$

Exact evaluation is $\#P$-hard at essentially every non-trivial point of the $(x,y)$ plane. Classical FPRAS exists only in restricted regions (high-temperature ferromagnetic case via Markov chain Monte Carlo), and an FPRAS is known to be NP-hard for roughly three quarters of the Tutte plane.

---

## What the paper does

Two main contributions, split across 82 pages:

**Part 1 (Algorithm, pp. 1–36).** Extends the [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL algorithm]] from braids to arbitrary planar graphs, and from unitary [[Algebraic Gate Design via Temperley-Lieb Representations|Temperley-Lieb representations]] to **non-unitary** ones. This covers the full Tutte plane — including the Potts model at any temperature, any number of colours, and any edge weights.

The core technical innovation: defining the [[Generalized Temperley-Lieb Algebra for Arbitrary Graphs|Generalized Temperley-Lieb algebra GTL(d)]], which allows an arbitrary number of strands (creation and annihilation). This removes the requirement that the input be a braid with fixed strand count, and — miraculously — also removes the need for the Markov property in the representation. Any representation of $\text{GTL}(d)$ works.

**Part 2 (BQP-completeness, pp. 36–82).** Proves BQP-hardness (and, with a modified problem formulation, BQP-completeness) for three classes of edge weights:
- **Unitary weights:** generalises the Jones polynomial case; proof follows [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes|Aharonov-Arad (2006)]]
- **Non-unitary complex weights:** requires proving density in $SL(n, \mathbb{C})$ instead of $SU(n)$, using [[Jørgensen Inequality for Non-Unitary Universality Seeding|Jørgensen's inequality]] from Möbius transformation theory
- **Non-unitary real weights:** density in $SL(n, \mathbb{R})$ / the orthogonal group, with separate obstacles

---

## The algorithm

### Step 1: From the Tutte polynomial to the Kauffman bracket

Given a planar graph $G$, construct its **medial graph** $L_G$ — a 4-regular graph obtained by encircling the facets of $G$ and placing a crossing vertex at each edge. The Kauffman bracket of $L_G$ relates to the Tutte polynomial via:

$$\langle L_G \rangle(d, u) = d^{-|V|} Z_G(d^2, du)$$

So approximating the Kauffman bracket (with $q = d^2$, $v = du$) is equivalent to approximating the Tutte polynomial.

### Step 2: The Generalized Temperley-Lieb algebra GTL(d)

The key algebraic tool. Unlike the standard $TL_n(d)$ algebra (which has a fixed number $n$ of strands), $\text{GTL}(d)$ is an infinite-dimensional algebra of diagrams connecting $n$ lower pegs to $m$ upper pegs, with no crossings, where two diagrams are equivalent if they're topologically equivalent or differ only by adding straight strands on the right. The loop value is $d$.

**Elementary tangles:** cups $A_i$ (local minimum at position $i$), caps $B_i$ (local maximum), and crossings $C_i(u)$ (weighted crossing between strands $i$ and $i+1$). A crossing with weight $u$ decomposes as:

$$C_i(u) = uX + Y$$

where $X$ connects the shaded areas and $Y$ disconnects them.

**The connection to the Kauffman bracket:** Any planar graph can be embedded as a tangle in $\text{GTL}(d)$. The Kauffman bracket equals the coefficient of the identity element:

$$\Psi_d(L_G) = \langle L_G \rangle(d, u) \cdot \mathcal{I}$$

This is the key departure from [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL]]: in the braid case, you need a representation with the Markov property. In the $\text{GTL}(d)$ setting, **any representation works**, because the Kauffman bracket is just the scalar multiplying the identity — not a trace satisfying special properties. See the trick card [[Generalized Temperley-Lieb Algebra for Arbitrary Graphs]] for why this matters.

### Step 3: The path model representation

The representation maps $\text{GTL}(d)$ elements to operators on the Hilbert space of paths on an auxiliary graph $F_\infty$ (the half-infinite line $1 - 2 - 3 - \ldots$). For paths of length $n$ starting at vertex 1, this gives a Hilbert space $H_n$.

The representation is determined by coefficients $a_{\ell,k}$ and $b_{\ell,k}$ (associated with local minima and maxima), which must satisfy:

$$b_{\ell,k} \cdot a_{\ell,k} = 1, \qquad \sum_{\ell \sim k} a_{\ell,k} b_{k,\ell} = d$$

These are set using the eigenvector $\bar{\pi}$ of $F_\infty$'s adjacency matrix with eigenvalue $d$:

$$a_{\ell,k} = \sqrt{\pi_\ell / \pi_k}, \qquad b_{\ell,k} = \sqrt{\pi_k / \pi_\ell}$$

where $\pi_1 = 1$, $\pi_2 = d$, $\pi_3 = d^2 - 1$, etc. The representation is Hermitian (and hence the crossing operators unitary) when all $\pi_i > 0$ — this happens for $d = 2\cos(\pi/k)$ or $d > 2$.

### Step 4: Implementing the algorithm

Given a "nicely embedded" planar graph (where sweeping a horizontal line meets one elementary tangle at a time), the algorithm:

1. Prepare $|1\rangle \in H_0$ (the 1-dimensional space of zero-length paths)
2. Apply the path-model image of each elementary tangle in sequence. Each operates on $O(\log |E|)$ qubits (3 local registers)
3. For non-unitary operators: use [[Non-Unitary Gate Implementation via Post-Selection|polar decomposition + ancilla post-selection]] — decompose $M = PU$, apply $U$ unitarily, apply $P/\|P\|$ via a controlled rotation on an ancilla, post-select on $|0\rangle$. The norm $\|M\|$ is booked as a factor in the approximation scale.
4. Estimate $\langle 1 | Q | 1 \rangle$ via the [[Hadamard Test for Trace Estimation|Hadamard test]]
5. The Tutte polynomial is $Z_G(q,v) = d^{|V|} \cdot \langle 1 | Q | 1 \rangle \cdot \Delta_{\text{alg}}$

### The approximation scale

$$\Delta_{\text{alg}} = q^{-|V|/2} \prod_{i=1}^{N} \|\rho(T_i)\|$$

— the product of the norms of all elementary tangle operators. For unitary representations (Jones polynomial case), all crossing operators have norm 1, so the scale is benign. For non-unitary representations, the product of norms can be exponentially large, making the additive approximation potentially trivial.

---

## Key results

| Result | Statement |
|---|---|
| **Theorem 1.1 (Algorithm)** | Efficient quantum algorithm for additive approximation of the multivariate Tutte polynomial at any point, for any planar graph with complex weights |
| **Theorem 1.2 (BQP-hardness)** | For a wide range of complex weights and $q$, additive approximation to scale $\Delta_{\text{hard}}$ is BQP-hard |
| **Theorem 1.3 (BQP-completeness)** | With input = graph + edge partition, the problem is BQP-complete for unitary and non-unitary parameters |

Examples of BQP-complete parameters:
- $q = 3$, unitary weights: $\{3/(e^{\pi i/3} - 1),\ 3/(1 - e^{-\pi i/3}),\ e^{\pi i/3} - 1,\ 1 - e^{-\pi i/3}\}$
- $q = 2i$, complex non-unitary weights: $\{100,\ -2i-100,\ 1,\ -1/2\}$
- $q = 3$, real non-unitary weights: $\{100,\ -103,\ 1,\ -1/2\}$

---

## The universality proof structure

### Encoding qubits into paths

Uses the **4-step encoding** (independently discovered by Kitaev and Wocjan-Yard):

$$|0\rangle = |12121\rangle \in H_4, \qquad |1\rangle = |12321\rangle \in H_4$$

An $n$-qubit state lives in the **legitimate subspace** $L_{4n} \subset H_{4n}$. Two-qubit gates are approximated by crossing operators $\sigma_i(u)$ acting on 8-step paths. The relevant subspace is $K = H_{8,1\to 1}$ — the 14-dimensional space of 8-step paths starting and ending at vertex 1 (listed diagrammatically in Fig. 10 of the paper).

### The crossing operators

$$\sigma_i(u) = \begin{cases} u\mathbb{1} + \Phi_i & \text{odd } i \\ \mathbb{1} + u\Phi_i & \text{even } i \end{cases}$$

where $\Phi_i = \rho(E_i)$ and $E_i^2 = dE_i$. Three parameter regimes:

| Type | Condition | Crossing operators | Density target |
|---|---|---|---|
| Unitary | $\|v + q\| = \|v\|$ (odd), $\|1 + v\| = 1$ (even) | Scalar × unitary | $SU(K)$ |
| Complex non-unitary | At least one weight/q not real, Jørgensen condition | General $GL$ | $SL(K, \mathbb{C})$ |
| Real non-unitary | All real, $q > 4\cos^2(\pi/5)$, Jørgensen condition | Real $GL$ | $SL(K, \mathbb{R})$ |

### The density proof: three stages

**Stage 1 — Seeding (density in 2D).** Show that normalised crossing operators $\hat{\sigma}_1, \hat{\sigma}_2$ generate a dense subgroup of $SU(2)$ (unitary case) or $SL(2, \mathbb{C})$/$SL(2, \mathbb{R})$ (non-unitary) on each 2×2 block.

- *Unitary case:* Use the classification of finite subgroups of $SO(3)$ — if the group were finite, it would have to be cyclic, dihedral, or one of $A_4, S_4, A_5$. The eigenvalue conditions rule out all five possibilities.
- *Non-unitary case:* Use [[Jørgensen Inequality for Non-Unitary Universality Seeding|Jørgensen's inequality]] — if the group is non-elementary (checked via Baribeau-Ransford) and satisfies $|\text{Tr}^2(X) - 4| + |\text{Tr}(XYX^{-1}Y^{-1}) - 2| < 1$, it must be non-discrete, hence dense in $SL(2, \mathbb{C})$ by Sullivan's theorem.

**Stage 2 — Building up (Bridge Lemma).** Given dense subgroups on $A$ and $B$ with $\dim A < \dim B$, and a "bridge" operator mixing them, the generated group is dense on $A \oplus B$. Applied iteratively with $\hat{\sigma}_3, \hat{\sigma}_4, \hat{\sigma}_5, \hat{\sigma}_6$ to climb from 2D to 14D.

**Stage 3 — Decoupling.** When the bridge lemma needs density on two subspaces that are generated by the *same* operators (so their actions might be correlated), the [[Decoupling via Iterated Commutators|Decoupling Lemma]] guarantees independence when $\dim A \neq \dim B$.

Both the [[Bridge Lemma for Tensor Product Universality|Bridge Lemma]] and Decoupling Lemma are reproved for the non-unitary setting ($SL(n, \mathbb{C})$ and $SL(n, \mathbb{R})$). The non-unitary Bridge Lemma proof (Appendix A.1) is constructive, building the target transformation via four subsidiary lemmas.

### From density to efficiency: Non-unitary Solovay-Kitaev

The standard [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev theorem]] applies to $SU(n)$. For $SL(n, \mathbb{C})$ and $SL(n, \mathbb{R})$, the paper proves a generalised version (Appendix B): given an $\varepsilon_0$-net over $B_R(M) \subset SL(M, \mathbb{F})$, any element can be approximated to accuracy $\delta$ using $\text{poly}(\log \delta^{-1})$ generators.

The key modification: the polar decomposition $\Delta = AP$ splits a near-identity $SL$ element into a unitary/orthogonal part $A$ and a positive-definite part $P$. Each is expressed as a product of group commutators (which cancels normalisation factors), and each commutator factor is approximated recursively. Error accumulation is controlled via the commutator error bound $\|[V,W] - [\tilde{V}, \tilde{W}]\| = O(\varepsilon\delta)$.

---

## Matching the approximation scales: the grouping technique

For unitary parameters, the algorithmic scale $\Delta_{\text{alg}}$ equals the hardness scale $\Delta_{\text{hard}}$ automatically (unitary operators have norm 1, so product-of-norms = norm-of-product). BQP-completeness follows immediately.

For non-unitary parameters, $\Delta_{\text{alg}} > \Delta_{\text{hard}}$ because the product of operator norms exceeds the norm of their product. The fix: [[Operator Grouping for Approximation Window Tightening|group neighbouring operators]] before applying them. The crossings approximating one two-qubit gate are grouped together; the group product is approximately unitary (norm $\approx 1$) on the relevant subspace, so the grouped scale matches $\Delta_{\text{hard}}$.

This requires modifying the problem: the input becomes a graph **plus a partition of its edges into groups**. The algorithm applies each group's product as a single operator. For $q = 3$, an additional technical complication arises: the approximation must be approximately unitary on **all** subspaces of $H_8^*$ (not just $K = H_{8,1\to 1}$), requiring simultaneous density proofs across 13 subspaces of dimensions 14 through 54 (Table 2). A generalised Solovay-Kitaev for direct sums of spaces handles this.

---

## The Potts model open problem

The most important limitation: **the universality proof does not cover the physically relevant Potts model parameters** (real positive edge weights, integer $q$, real temperature). The obstruction is structural: the Potts partition function is always positive for physical parameters, but BQP-hardness would require encoding arbitrary unitaries — including those whose matrix elements are negative. Since no graph has a negative Potts partition function, BQP-hardness seems impossible to prove with these methods.

The paper provides only weak evidence for non-triviality: the algorithm distinguishes between different edge-weight assignments on the line graph (where the exact answer is classically computable). Whether the quantum approximation of the physical Potts partition function is BQP-hard, classically easy, or intermediate remains open — arguably the most significant open question raised by this line of work.

---

## Comparison with prior work

| Paper | Scope | Representations |
|---|---|---|
| [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes\|AJL (2006)]] | Braids only, roots of unity | Unitary $TL_n(d)$ reps |
| [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes\|Aharonov-Arad (2006)]] | BQP-hardness for Jones, all polynomial $k$ | Unitary path model |
| **This paper (2007)** | Arbitrary planar graphs, full Tutte plane | Unitary and non-unitary $\text{GTL}(d)$ reps |
| [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes\|Mann-Bremner (2019)]] | Classical FPTAS for complex Ising on bounded-degree | N/A (classical) |

The step from AJL to this paper: braids → arbitrary planar graphs, unitary → non-unitary, $TL_n(d)$ → $\text{GTL}(d)$, Markov property required → any representation works.

---

## Limits / caveats

- The additive approximation window scales with $\prod_e \|M_e\|$, which can be exponentially large for non-unitary representations. The edge-partition trick is needed for BQP-completeness in the non-unitary case — somewhat artificial.
- The physical Potts parameters are **not** covered by the universality proof, and the structural argument (positivity of the partition function) suggests these methods cannot reach them.
- Restricted to **planar graphs** — the TL structure requires planarity. Non-planar extensions are left as an open problem.
- The approximation scale depends on the graph embedding (edge ordering), and no efficient algorithm for optimising the embedding is known.
- For BQP-completeness at $q = 3$ (non-unitary), the proof requires a brute-force computer search over the 13 subspaces of $H_8^*$ — the characterisation of which points are BQP-complete is incomplete.

---

## Reusable ideas

1. **[[Generalized Temperley-Lieb Algebra for Arbitrary Graphs]]:** The $\text{GTL}(d)$ algebra with variable strand count replaces $TL_n(d)$. The Kauffman bracket becomes the scalar coefficient of the identity (not a Markov trace), so any representation works. This extends TL-based algorithms from braids to all planar graphs.

2. **[[Non-Unitary Gate Implementation via Post-Selection]]:** Implement a non-unitary operator $M$ via polar decomposition $M = PU$, apply $U$ unitarily, and $P/\|P\|$ via ancilla rotation + post-selection. Success probability $1/\|M\|^2$ per step.

3. **[[Graph-to-TL-Product Mapping for Tutte Polynomial]]:** Any planar graph's Tutte polynomial can be expressed as a product of $\text{GTL}(d)$ elements via its medial graph embedding.

4. **[[Operator Grouping for Approximation Window Tightening]]:** When applying a sequence of non-unitary operators, grouping consecutive operators and applying their classical product as one reduces the approximation scale from $\prod \|M_i\|$ to $\prod \|M_{g_j}\|$ where $M_{g_j}$ is the group product. Useful whenever product-of-norms is much larger than norm-of-product.

5. **[[Jørgensen Inequality for Non-Unitary Universality Seeding]]:** Uses Jørgensen's inequality from Möbius transformation theory to prove that non-unitary generators produce a dense subgroup of $SL(2, \mathbb{C})$ — the "seed" for building universality via the Bridge and Decoupling Lemmas.

---

## References within this paper

- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|Aharonov, Jones & Landau (2006)]] — the algorithm this paper extends
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes|Aharonov & Arad (2006)]] — universality proof for Jones (unitary case); Bridge and Decoupling Lemmas originated here
- Jones (1985) — discovery of the Jones polynomial
- Kauffman (1987) — bracket polynomial / state sum formulation; the Kauffman bracket used throughout
- Tutte (1947) — the Tutte polynomial
- Potts (1952) — the $q$-state Potts model
- Sokal (2005) — the multivariate Tutte polynomial reference; notation and treatment followed closely
- Freedman, Kitaev, Larsen & Wang (2002) — topological quantum computation; original universality proof via TQFT
- Jørgensen (1976) — inequality for discrete Möbius groups; used for non-unitary seeding
- Sullivan (1985) — non-elementary non-discrete subgroups of $SL(2, \mathbb{C})$ are dense; the other half of the seeding argument
- Jaeger, Vertigan & Welsh (1990) — $\#P$-hardness of exact Tutte polynomial evaluation
- Goldberg & Jerrum (2006) — inapproximability of the Tutte polynomial (NP-hardness of FPRAS for ~3/4 of the Tutte plane)
- Fortuin & Kasteleyn (1972) — the random cluster representation connecting the Potts model to the Tutte polynomial
- Kitaev (2005, private communication) / Wocjan & Yard (2006) — the 4-step path encoding

---

## Cross-links

### Paper notes
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]] — the Jones polynomial algorithm this paper generalises
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]] — BQP-hardness for the unitary (Jones) case
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]] — classical algorithms for related partition functions
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — IQP hardness connects to partition function approximation

### Trick cards
- [[Generalized Temperley-Lieb Algebra for Arbitrary Graphs]] — the algebraic framework
- [[Non-Unitary Gate Implementation via Post-Selection]] — implementing non-unitary operators
- [[Graph-to-TL-Product Mapping for Tutte Polynomial]] — the graph ↔ TL product correspondence
- [[Operator Grouping for Approximation Window Tightening]] — the grouping technique for BQP-completeness
- [[Jørgensen Inequality for Non-Unitary Universality Seeding]] — the non-unitary density seed
- [[Hadamard Test for Trace Estimation]] — used for estimating $\langle 1 | Q | 1 \rangle$
- [[Algebraic Gate Design via Temperley-Lieb Representations]] — the underlying representation approach
- [[Bridge Lemma for Tensor Product Universality]] — building universality from lower-dimensional seeds
- [[Decoupling via Iterated Commutators]] — separating correlated actions on different subspaces
- [[Solovay-Kitaev Recursive Gate Compilation]] — extended to non-unitary groups in this paper
