> **Source:** Alexander Schmidhuber, Seth Lloyd, *Complexity-theoretic limitations on quantum algorithms for topological data analysis*, arXiv:2209.14286, PRX Quantum **4**, 040349 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2209.14286) · [PRX Quantum](https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.4.040349)
> **Tags:** #topological-data-analysis #Betti-numbers #complexity-theory #NP-hard #sharp-P #quantum-advantage #negative-result

---

## The computational problem

Two formal problems:

- **Betti:** Given a clique complex $S$ (via vertices and edges) and integer $k$, output $\beta_k$.
- **Homology:** Given a clique complex $S$ and integer $k$, decide whether $\beta_k > 0$.

## What the paper does

**Negative result.** Shows that computing or even approximating Betti numbers is intractable *even for quantum computers*:

1. **Computing Betti numbers exactly is #P-hard** (Theorem 1), even for clique-dense complexes.
2. **Deciding $\beta_k > 0$ is NP-hard** (Theorem 2), even for clique-dense complexes. This immediately implies that approximating Betti numbers to any multiplicative error is NP-hard.

Since quantum computers are not believed to solve NP-hard or #P-hard problems, these results show the [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|LGZ algorithm]] cannot provide an exponential speedup in the worst case. The exponential advantage is limited to specific promise problems (e.g., when the clique fraction, spectral gap, and Betti number size are all favourable).

The paper also shows that the LGZ algorithm achieves only a quadratic (Grover-like) speedup for asymptotically almost all random Vietoris-Rips complexes.

My assessment: this is the paper that tempered the TDA enthusiasm. The result is clean and the implications are clear — the quantum TDA speedup is not a generic exponential advantage but a conditional one. The hardness lies not in the QPE/kernel-estimation step but in the state preparation (constructing the simplex state is #P-hard by itself). If you're given an oracle for sampling simplices, the exponential advantage can be recovered.

## The proofs

### #P-hardness (Theorem 1)

The proof reduces #SAT to computing the Euler characteristic $\chi(S) = \sum_{k=0}^{n-1} (-1)^k |S_k|$, which equals $\sum_{k=0}^{n-1} (-1)^k \beta_k$ by the Euler-Poincaré formula.

Given a SAT instance $K$ with variables $X_1, \ldots, X_n$ and clauses $C_1, \ldots, C_s$:

1. For each variable $X_i$, create 3 vertices: $t_i, f_i, p_i$ (true, false, padding)
2. Connect all pairs within each group (so $\{t_i, f_i, p_i\}$ is a 3-clique for each $i$)
3. Add a vertex $c_j$ for each clause, connected to all vertices except $t_i$ when $X_i$ appears in $C_j$ and $f_i$ when $\neg X_i$ appears in $C_j$

The Euler characteristic of the resulting clique complex equals $(-1)^{n-1} \times (\text{number of satisfying assignments}) + 1$. Since $\chi(S)$ is computable from the Betti number vector, computing all Betti numbers is #P-hard.

### NP-hardness for clique-dense complexes (Theorem 2)

By restricting to 3-SAT instances with clause-to-variable ratio $s/n \leq c$ (still NP-complete), the constructed graph has edge density $\geq 1/2 - O(1/n)$, making the complex clique-dense. So even in the regime where the LGZ algorithm runs most efficiently, deciding whether a Betti number is nonzero is NP-hard.

### Average-case analysis on random Vietoris-Rips complexes

For the Vietoris-Rips complex on $n$ random points in $\mathbb{R}^d$ at scale $\epsilon$:

- Betti numbers satisfy $\beta_k \in O(n)$ for asymptotically almost all inputs (by the Kahle-Meckes theorem)
- The number of $k$-simplices $|S_k|$ grows as $\binom{n}{k+1} \cdot p^{\binom{k+1}{2}}$
- The quantum lower bound is $T_q = \Omega\left(\sqrt{\binom{n}{k+1}/\beta_k}\right)$

This gives only a **quadratic speedup** over the classical $O(|S_k|)$ for most inputs.

## Key results

| Result | Statement | Implication |
|---|---|---|
| Theorem 1a | Betti is #P-hard | No polynomial quantum algorithm for exact Betti numbers |
| Theorem 1b | Betti is #P-hard even for clique-dense complexes | Hardness persists in the LGZ algorithm's best regime |
| Theorem 2a | Homology is NP-hard | No polynomial quantum algorithm for multiplicative approximation |
| Theorem 2b | Homology is NP-hard for clique-dense complexes | NP-hardness in the best quantum regime |
| Section V | Quadratic speedup for almost all random VR complexes | Generic advantage is Grover-like, not exponential |

## Comparison with prior work

| Aspect | Gyurik-Cade-Dunjko (2022) | This paper |
|---|---|---|
| Direction | Evidence FOR quantum advantage | Evidence AGAINST generic exponential advantage |
| Result | LLSD (generalisation) is DQC1-hard | Betti number computation is #P-hard / NP-hard |
| Scope | Applies to general sparse matrices | Applies specifically to clique complexes |
| Conclusion | TDA resists generic dequantization | But even quantum computers can't solve TDA efficiently in general |

These results are complementary, not contradictory: DQC1-hardness of the generalised problem means classical dequantization fails, while NP-hardness of the specific problem means quantum algorithms also struggle in the worst case. Exponential advantage exists only in a specific parameter regime.

## Limits / caveats

1. **Promise problems escape.** The hardness results are worst-case. For specific promise problems (e.g., "the complex has clique fraction $\geq 1/\text{poly}(n)$, spectral gap $\geq 1/\text{poly}(n)$, and $\beta_k \geq |S_k|/\text{poly}(n)$"), quantum algorithms may still have exponential advantage.

2. **Oracle model recovers advantage.** If given an oracle for uniform simplex sampling (rather than vertex/edge input), the exponential speedup returns. This is relevant for problems where simplices are specified directly (e.g., hypergraph structures in social networks).

3. **Concurrent work.** Crichigno & Kohler (2022, arXiv:2209.11793) independently proved that Homology is QMA₁-hard (stronger than NP-hard), using supersymmetric quantum systems.

## Reusable ideas

This is primarily a complexity theory paper — it has no new algorithms, so no trick cards. The main transferable idea is the SAT-to-Euler-characteristic reduction, which is a neat connection between Boolean satisfiability and simplicial topology, but it's more of a proof technique than a reusable algorithmic component.

## References within this paper

- **[[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|Lloyd, Garnerone, Zanardi (2016)]]** — The algorithm whose limitations this paper establishes.
- **[[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes|Gyurik, Cade, Dunjko (2022)]]** — DQC1-hardness (complementary positive result).
- **[[Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) — Paper Notes|Roune & Sáenz-de-Cabezón (2013)]]** — #P-completeness of Euler characteristic computation for abstract simplicial complexes (facet input). This paper proves it for clique complexes (vertex/edge input).
- **Adamaszek & Stacho** — Prior NP-hardness for co-chordal graphs. Theorem 2 strengthens to clique-dense.
- **Crichigno & Kohler (2022)** — QMA₁-hardness of Homology (simultaneous independent work, stronger result).
- **Kahle & Meckes (2013)** — Betti number bounds for random geometric complexes.

## Cross-links

### Paper notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] — The algorithm whose limitations are established
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] — Complementary positive result (DQC1-hardness)
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Quantifies the advantage regime precisely
- [[Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) — Paper Notes]] — Classical complexity baseline
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — BQP₁-hardness for persistence (Schmidhuber is coauthor on both)
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]] — Extension to persistent setting

### Trick cards
- [[Simplex-to-Qubit Encoding for Simplicial Complexes]] — Encoding underlying the algorithm analysed
- [[Kernel Dimension Estimation via QPE on Mixed States]] — Core technique whose limitations are characterised
