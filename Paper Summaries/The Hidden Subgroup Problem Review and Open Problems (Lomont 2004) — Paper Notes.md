# The Hidden Subgroup Problem Review and Open Problems (Lomont 2004) — Paper Notes

> **Source:** Chris Lomont, *The hidden subgroup problem - review and open problems*, arXiv:quant-ph/0411037
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0411037)
> **Tags:** #hidden-subgroup-problem #survey #nonabelian

---

## TL;DR

Lomont's review is a long mathematical guide to the hidden subgroup problem as of late 2004. It develops the quantum circuit model, the finite abelian HSP algorithm, Fourier transforms over finite groups, the standard nonabelian Fourier-sampling approach, and then catalogs known positive and negative evidence for nonabelian HSP. Its lasting value is not a single new algorithm, but a set of diagnostic questions: Can the group be Fourier transformed efficiently? Does weak or strong Fourier sampling separate the candidate subgroups? If the quantum samples contain enough information, is there an efficient classical reconstruction step? Use it as vocabulary and early landscape, not as current status on constants or post-2004 results.

## Metadata

- **Author:** Chris Lomont
- **Year:** 2004
- **arXiv:** [quant-ph/0411037](http://arxiv.org/abs/quant-ph/0411037)
- **Source text read:** `/tmp/hsp_zoo_all/zoo69_quant-ph_0411037.txt`
- **Algorithm Zoo:** #69 ✓
- **Status:** Paper note created from full extracted text.

## Scope

The review aims to unify notation and give proofs or concrete algorithm statements for much of the early HSP literature. It covers:

- the quantum computation model and query model;
- the finite abelian HSP and its standard algorithm;
- fast Fourier transforms and QFT circuits, including $\mathbb Z_N$ and finite abelian products;
- representation theory for nonabelian groups;
- the general finite-group Fourier transform;
- weak and strong quantum Fourier sampling;
- known nonabelian cases and related group-algorithm settings, including dihedral, metacyclic/semidirect, solvable-group structural tasks, and black-box groups;
- links to graph isomorphism and hidden shift;
- technical appendices on cyclic QFT approximation, graph reductions, quantum mechanics basics, random generation, and gcd probabilities.

Some entries in this survey have since been sharpened or superseded by later notes in this vault, especially the Kuperberg/Regev dihedral-HSP line and the extraspecial/nil-2 positive results.

## Abelian HSP framework

For a finite abelian group $G$ and subgroup $H$, the algorithm prepares a coset state, applies the QFT over $G$, and samples a random character from $H^\perp$. Repeating $O(\log |G|)$ times gives enough generators for $H^\perp$, from which $H$ is recovered via classical linear algebra such as Smith normal form.

The review gives explicit bounds and implementation details, including:

- decomposition of finite abelian groups into cyclic factors;
- character-group identification $\widehat G \cong G$;
- QFT over $\mathbb Z_N$;
- efficient QFT over $\mathbb Z_{2^n}$ using Hadamards and controlled phase rotations;
- approximate QFT bounds in the appendices.

## General finite-group framework

For a finite group $G$, the nonabelian Fourier transform maps group elements into matrix blocks indexed by irreducible representations. The standard HSP experiment prepares a coset state and measures after applying the Fourier transform.

The review distinguishes two sampling modes:

- **Weak Fourier sampling:** measure only the representation label.
- **Strong Fourier sampling:** also measure row/column data after choosing bases in representation spaces.

This leads to a diagnostic split:

1. Efficient QFT exists for the group family.
2. The measurement distribution has enough statistical separation for the hidden subgroups under consideration.
3. The resulting samples can be post-processed efficiently.

Failure at any step can block an HSP algorithm even if the query complexity is polynomial.

## Nonabelian results and barriers

The review records several themes that remain useful as a map of HSP methods:

- Dihedral HSP connects to hidden shift, subset-sum-like postprocessing, and lattice problems.
- Graph isomorphism reduces to HSP over symmetric groups, but standard Fourier-sampling approaches face strong evidence of failure.
- Some group families admit efficient QFTs, but efficient QFT alone does not imply an efficient HSP algorithm.
- Black-box group algorithms solve related tasks such as group order, decomposition of finite abelian black-box groups, and solvable-group computations.
- Normal subgroup reconstruction is easier than arbitrary subgroup reconstruction in several settings.

## Reusable diagnostics extracted as trick cards

For this review I only made a trick card for framework-level reuse, not for generic survey items:

- Nonabelian QFS Reconstruction Checklist — a diagnostic for deciding where a proposed nonabelian HSP approach might fail: transform, statistical distinguishability, basis choice, and reconstruction.

The abelian material is already covered by existing cards such as [[HSP as Algorithmic Template (Abelian)]] and [[Coset Sampling via Fourier Transform]].

## Connections

- Provides background for [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill (2004)]], especially the distinction between sample/query information and efficient extraction.
- Gives context for [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] and [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]].
- Sets up the landscape in which [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes|Ivanyos-Sanselme-Santha (2007)]] and [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) — Paper Notes|Ivanyos-Sanselme-Santha (2008)]] are later positive examples.
- Adjacent to [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes|Childs-van Dam (2010)]], a later survey with updated perspective.

## Why it matters

The review is useful as an HSP map. It separates three ideas that are often conflated: efficient implementation of the Fourier transform, information-theoretic distinguishability of hidden subgroups, and efficient classical recovery from samples. That separation helps explain why abelian HSP is solved, why some structured nonabelian families are tractable, and why symmetric-group HSP remains hard for standard Fourier-sampling routes.

## Caveats

- The paper is a 2004 review, so it predates Kuperberg's final journal version, the extraspecial and nil-2 algorithms, later measurement lower bounds, and modern HSP surveys.
- It is broad and tutorial in places; not every section should become a trick card.
- Some constants and proof details are presented for exposition rather than as a polished implementation guide.
