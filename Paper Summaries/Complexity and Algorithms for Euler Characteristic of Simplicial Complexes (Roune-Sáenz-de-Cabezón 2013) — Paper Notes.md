> **Source:** Bjarke Hammersholt Roune, Eduardo Sáenz-de-Cabezón, *Complexity and algorithms for Euler characteristic of simplicial complexes*, arXiv:1112.4523, Journal of Symbolic Computation **50**, 170–196 (2013)
> **Links:** [arXiv](https://arxiv.org/abs/1112.4523) · [Journal](https://doi.org/10.1016/j.jsc.2012.07.005)
> **Tags:** #simplicial-complex #Euler-characteristic #sharp-P #classical-complexity #algorithms

---

## The computational problem

Given an abstract simplicial complex $\Delta$ specified by its **vertices and facets** (maximal simplices), compute the reduced Euler characteristic:

$$\tilde{\chi}(\Delta) = -\sum_{\sigma \in \Delta} (-1)^{|\sigma|} = -f_{-1} + f_0 - f_1 + f_2 - \cdots$$

where $f_i$ is the number of $i$-dimensional faces. By the Euler-Poincaré formula, $\tilde{\chi}(\Delta) = \sum_k (-1)^k \beta_k$, so computing the Euler characteristic is at most as hard as computing all Betti numbers.

## What the paper does

1. **Proves computing Euler characteristic is #P-complete** (Theorem 1) when the complex is specified by facets and vertices. This settles an open question of Kaibel and Pfetsch. The companion result: deciding $\tilde{\chi}(\Delta) = 0$ is co-NP-hard and not in NP unless #P = NP.

2. **Presents two new practical algorithms** (BCRT and DBMS) derived from combinatorial commutative algebra, described both algebraically and combinatorially. Both are divide-and-conquer algorithms based on the splitting identity $\tilde{\chi}(I) = \tilde{\chi}(I : p) + \tilde{\chi}(I + \langle p \rangle)$.

3. **Experiments** showing these algorithms are much faster than previous implementations for practical instances.

This is a **purely classical paper** — no quantum computing. Its relevance to quantum TDA is as the **classical complexity baseline**: it establishes that even computing the Euler characteristic (a weaker invariant than individual Betti numbers) is #P-complete, confirming the exponential classical hardness that quantum TDA algorithms aim to circumvent.

My assessment: a clean complexity theory result plus practical algorithms. The #P-completeness proof is elegant — it reduces #SAT to Euler characteristic via independent sets of a carefully constructed graph, anticipating the similar SAT-to-Euler-characteristic reduction later used by [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes|Schmidhuber & Lloyd (2023)]] for clique complexes.

## The proof

### #P-completeness (Theorem 1)

The proof goes #SAT $\to$ IndepSum $\to$ EulerChar:

**IndepSum:** Given a graph $G$, compute the parity sum $P(I) = \sum_{S \in \mathcal{I}(G)} (-1)^{|S|}$ over all independent sets $\mathcal{I}(G)$.

**IndepSum $\to$ EulerChar:** For graph $G$ with vertex set $V$, define $\Delta$ with facets being complements of edges. Then faces of $\Delta$ correspond to dependent sets of $G$, and $P(\mathcal{I}) = (-1)^n \tilde{\chi}(\Delta)$.

**#SAT $\to$ IndepSum:** Given a SAT formula with variables $v_1, \ldots, v_n$ and clauses $c_1, \ldots, c_s$:
- Create vertices $T_i, F_i, D_i$ for each variable (3-clique per variable)
- Create vertex $C_j$ for each clause, with edges encoding literal-clause incidence
- The maximal independent sets contained in $\{T_i, F_i\}$ correspond exactly to satisfying assignments
- All other independent sets cancel in the parity sum (via a bijection argument)

### Decision problems (Theorems 3–4)

- Deciding $\tilde{\chi}(\Delta) = 0$ is co-NP-hard
- Deciding $\tilde{\chi}(\Delta) > 0$ is #P-hard
- Neither lies in NP unless #P collapses to NP

## The algorithms

Both algorithms are divide-and-conquer based on:

$$\tilde{\chi}(I) = \tilde{\chi}(I : p) + \tilde{\chi}(I + \langle p \rangle)$$

where $I$ is a squarefree monomial ideal and $p$ is a pivot monomial. The two algorithms differ in pivot selection:

- **BCRT (Bigatti-Conti-Robbiano-Traverso style):** Splits $I$ into $I : p$ and $I + \langle p \rangle$. Pivot $p$ is chosen heuristically to maximise simplification.
- **DBMS (Bayer-Stillman style):** Picks $p \in \min(I)$ (a minimal generator), splits into $\langle \min(I) \setminus \{p\} \rangle$ and its colon.

Base case: if $I$ doesn't have full support, $\tilde{\chi}(I) = 0$.

## Key results

| Result | Statement |
|---|---|
| Theorem 1 | Computing $\tilde{\chi}(\Delta)$ from vertices and facets is #P-complete |
| Theorem 3 | Deciding $\tilde{\chi}(\Delta) = 0$ is co-NP-hard |
| Theorem 4 | Deciding $\tilde{\chi}(\Delta) > 0$ is #P-hard |
| Section 6 | BCRT/DBMS algorithms outperform prior implementations by large margins |

## Comparison with quantum TDA results

| Problem | Classical complexity (this paper) | Quantum complexity (later papers) |
|---|---|---|
| Euler characteristic | #P-complete (facet input) | Computable from all Betti numbers; quantum doesn't help with #P |
| Individual $\beta_k$ | At least as hard as $\chi$ | #P-hard exact, NP-hard approximate ([[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes|Schmidhuber-Lloyd 2023]]) |
| Normalised $\beta_k/|S_k|$ | Classical LLSD algorithms: $O(n \cdot \chi_k)$ | Quantum: $\tilde{O}(\text{poly}(n))$ for clique-dense ([[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|LGZ 2016]]) |

The quantum advantage exists for the *normalised approximation* problem, which this paper doesn't address. The #P-completeness here is for the *exact* computation problem with *facet input*.

## Limits / caveats

1. **Input model matters.** This paper uses facet input (maximal simplices). [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes|Schmidhuber & Lloyd (2023)]] proved #P-hardness for vertex/edge input (clique complexes), which is the natural input for TDA.

2. **Euler characteristic vs. Betti numbers.** Computing $\chi$ is necessary but not sufficient for computing individual $\beta_k$. The #P-hardness of $\chi$ doesn't directly imply #P-hardness of individual Betti numbers (though this was later shown separately).

## Reusable ideas

No quantum tricks — this is a classical paper. The SAT-to-Euler-characteristic reduction via independent set parity sums is a proof technique reused by subsequent papers in the quantum TDA complexity story.

## References within this paper

- **Kaibel & Pfetsch** — Posed the open questions answered here (complexity of Euler characteristic and $f$-vector).
- **Bigatti, Conti, Robbiano, Traverso** — Original Hilbert series algorithm that inspired the BCRT approach.
- **Bayer & Stillman** — Original Hilbert series algorithm that inspired the DBMS approach.

## Cross-links

### Paper notes
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] — Extends the #P-hardness to clique complexes and proves NP-hardness of approximation
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] — Quantum algorithm that aims to circumvent this classical hardness
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] — DQC1-hardness of the quantum-solvable variant
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Includes classical PIMC algorithm as partial dequantization
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — BQP$_1$-hardness for persistence; broader context for classical vs quantum complexity of simplicial topology

### Trick cards
(No trick cards — purely classical complexity/algorithms paper)
