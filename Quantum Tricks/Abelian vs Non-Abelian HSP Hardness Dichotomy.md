# Abelian vs Non-Abelian HSP Hardness Dichotomy

> **Source:** Childs and van Dam, arXiv:0812.0380, §II–IV; Ettinger-Høyer-Knill (2004); Kuperberg (2005); Regev (2002)
> **Tags:** #trick #hidden-subgroup-problem #nonabelian #hardness #graph-isomorphism #lattice #design-heuristic

## What it does

Gives a decision procedure for whether a new HSP instance is likely tractable or hard, based on the algebraic structure of the group. Separates the query complexity question (always polynomial) from the computational complexity question (group-dependent, and often exponential for non-abelian groups).

## The trick

**Tier 1 — Polynomial time (tractable):**
- $G$ is any **finite abelian group**: use [[HSP as Algorithmic Template (Abelian)|the abelian template]]. Polynomial queries + QFT = efficient algorithm.
- $G$ is **solvable** ([[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous 2001]]): apply the abelian algorithm recursively along a subnormal chain $G = G_0 \triangleright G_1 \triangleright \cdots \triangleright G_k = \{e\}$ with abelian quotients. Each level is an abelian HSP.
- $G$ is a **semidirect product** $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$ with **constant $r$** ([[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam 2005]]): the pretty good measurement on $r$ coset state copies works and reduces to a tractable matrix sum problem over $\mathbb{F}_p$.

**Tier 2 — Subexponential (hard but not classical-exponential):**
- $G$ is the **dihedral group** $D_N$: $2^{O(\sqrt{\log N})}$ time via Kuperberg's sieve ([[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg 2005]]), with polynomial-space variant ([[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev 2004]]). No polynomial-time algorithm is known.

**Tier 3 — Likely hard (classical-exponential barrier):**
- $G = D_N$ and you need polynomial time: [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] shows dihedral HSP $\Rightarrow$ SVP/CVP hardness. A polynomial-time quantum algorithm would imply polynomial-time quantum algorithms for worst-case lattice problems — considered very unlikely.
- $G = S_n$ (symmetric group, graph isomorphism): standard coset sampling is provably insufficient for exponentially many subgroups (Moore-Russell-Schulman). Single-copy measurements can't distinguish subgroups. Multi-copy PGM reduces to average-case subset sum — believed hard.

**The query complexity baseline**: for any finite group, $O(\log^4 |G|)$ quantum queries suffice ([[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill 2004]]). The bottleneck is never in obtaining the quantum information — it's always in processing the coset states.

**Decision heuristic when encountering a new HSP instance:**
1. Is $G$ abelian? → polynomial time
2. Is $G$ solvable? → polynomial time
3. Does $G$ have structure connecting to lattice problems? → likely subexponential-only
4. Does $G$ look like $S_n$ (exponentially many candidate subgroups, no useful representation theory)? → likely hard

## When to reach for it

- Deciding whether to pursue a quantum algorithm for a new algebraic / number-theoretic problem
- Evaluating whether the "HSP reduction" for a problem like graph isomorphism can yield an efficient algorithm
- Understanding why certain problems (GI, subset sum) seem quantum-hard despite quantum algorithms existing for structurally similar ones
- Explaining the landscape of quantum speedups in algebra to non-experts

## Complexity

This is a design heuristic, not a runnable algorithm. The actual complexity of each tier is:
- Tier 1: $\text{poly}(\log |G|)$ time and queries
- Tier 2: $2^{O(\sqrt{\log |G|})}$ time, $\text{poly}(\log |G|)$ space
- Tier 3: no known polynomial-time algorithm; presumed exponential

## Caveat

The dichotomy is not exhaustive. There may be groups that are non-solvable but tractable for HSP (no example is known), or that fall between Tier 2 and Tier 3. The dihedral group is a special case because of its specific lattice connection — not all subexponential cases have this hardness evidence.

Also: "the group is non-abelian" does not automatically mean hard. The solvable group case (Tier 1) is a large and important class of non-abelian groups that are fully tractable.

## Related notes

- [[Hidden Subgroup Problem]] — concept hub
- [[HSP as Algorithmic Template (Abelian)]] — the tractable case in detail
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]] — what happens when you push beyond single-copy measurements
- [[Coset Sampling via Fourier Transform]] — the fundamental subroutine in all cases
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] — the survey
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] — query complexity is never the bottleneck
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]] — lattice hardness connection
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — Tier 1 non-abelian case
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
