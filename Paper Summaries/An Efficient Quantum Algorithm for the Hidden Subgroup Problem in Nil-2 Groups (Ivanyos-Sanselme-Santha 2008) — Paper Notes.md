# An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) — Paper Notes

> **Source:** Gábor Ivanyos, Luc Sanselme, and Miklos Santha, *An efficient quantum algorithm for the hidden subgroup problem in nil-2 groups*, arXiv:0707.1260
> **Links:** [arXiv](https://arxiv.org/abs/0707.1260)
> **Tags:** #hidden-subgroup-problem #nonabelian #nilpotent-groups

---

## TL;DR

This paper extends the extraspecial-group HSP algorithm to all finite nilpotency-class-at-most-2 groups. It first reduces HSP in constant-class nilpotent groups to the case of $p$-groups of exponent $p$ with hidden subgroup either trivial or cyclic of order $p$. In that reduced case, the quantum part builds a hiding procedure for $HG'$ using automorphism-controlled actions. The phase-cancellation condition becomes a homogeneous system of $d$ quadratic and $d$ linear equations over $\mathbb Z_p$, where $d = \dim_{\mathbb Z_p} G'$. Chevalley-Warning gives existence, and the paper gives an efficient search procedure.

## Metadata

- **Authors:** Gábor Ivanyos, Luc Sanselme, Miklos Santha
- **Year:** 2008 conference version; arXiv 2007
- **Venue:** LATIN 2008
- **arXiv:** [0707.1260](http://arxiv.org/abs/0707.1260)
- **Source text read:** `/tmp/hsp_zoo_all/zoo57_0707.1260.txt`
- **Algorithm Zoo:** #57 ✓
- **Status:** Paper note created from full extracted text.

## Problem setting

A nil-2 group is a group $G$ with

$$G' \leq Z(G).$$

The paper proves efficient quantum solvability of HSP in such groups. The central commutator subgroup is higher-dimensional than in extraspecial groups, so a single central phase coordinate is no longer enough. The paper treats the derived group as

$$G' \cong \mathbb Z_p^d,$$

and the quotient-like representative group as

$$G/G' \cong \mathbb Z_p^m.$$

## Main result

Theorem 1 states that if $G$ is a nil-2 group and $f$ hides $H \leq G$, then $H$ can be found by an efficient quantum procedure.

The proof is in two layers:

1. A classical reduction for nilpotent groups of constant class.
2. A quantum algorithm for nil-2 $p$-groups of exponent $p$.

## Classical reduction

Theorem 2 reduces HSP in any class of constant-nilpotency groups closed under subgroups and factor groups to the case where:

- the group is a $p$-group;
- the group has exponent $p$;
- the hidden subgroup is either trivial or order $p$.

The reduction uses refined polycyclic presentations. The key substeps are:

- decompose a nilpotent group into Sylow subgroups;
- iteratively find order-$p$ pieces of the hidden subgroup, restarting in normalizers/quotients when a generator is found;
- reduce to the exponent-$p$ subgroup $G^*$ of elements with order $1$ or $p$.

For nil-2 groups, Corollary 1 gives the specific reduction to nil-2 $p$-groups of exponent $p$ with hidden subgroup trivial or cyclic of order $p$.

This reduced case is enough because the classical group-theoretic preprocessing can peel off order-$p$ generators of the hidden subgroup one at a time, updating the problem in normalizers or quotients until the whole subgroup is generated.

## Nil-2 exponent-p structure

In the reduced case, each element has a unique representation

$$g = x_1^{e_1}\cdots x_m^{e_m}z_1^{f_1}\cdots z_d^{f_d},$$

where $G/G' \cong \mathbb Z_p^m$ and $G' \cong \mathbb Z_p^d$.

For $j \in \mathbb Z_p$, maps $\phi_j$ are defined by sending generators $x_i \mapsto x_i^j$ and extending to $G$. They satisfy:

$$\phi_j(z) = z^{j^2}\quad (z \in G'),$$

and for each $g \in G$ there is a unique $z_g \in G'$ such that

$$\phi_j(g) = g^j z_g^{j-j^2}.$$

These formulas are the whole reason the algorithm can cancel phases by solving low-degree equations.

## Quantum part

Theorem 3 reduces finding $H$ to building a quantum hiding procedure for $HG'$ on the abelian representative group $\overline G \cong G/G'$.

Theorem 4 builds that hiding procedure. It prepares states

$$|aHG'_u\rangle,$$

where $u \in \mathbb Z_p^d$ labels a Fourier character of $G'$. These states are eigenvectors for right actions by $\phi_j(g)$ when $g \in HG'$.

Taking $n$ copies gives

$$|\Psi^{a,u,j}_g\rangle = \bigotimes_{i=1}^n |a_iHG'_{u_i}\cdot \phi_{j_i}(g)\rangle.$$

To make this invariant under all $g \in HG'$, it is enough to find a nonzero $j = (j_1,\ldots,j_n)$ satisfying

$$\sum_i u_i j_i^2 = 0_d,\qquad \sum_i u_i j_i = 0_d.$$

The first equation cancels phases from $G'$, and the second cancels the $H$-dependent part after rewriting $j_i-j_i^2$.

## Chevalley-Warning witness search

Chevalley-Warning implies nonzero solutions once the number of variables is larger than the total degree. The paper goes further: it gives a polynomial-time way to find a solution by taking

$$n = \frac{(d+1)^2(d+2)}{2}.$$

The search proceeds by:

1. solving blocks of $d$ homogeneous quadratic equations in $(d+1)(d+2)/2$ variables;
2. using row operations, quadratic-residue tests, modular square roots, and induction on $d$;
3. combining $d+1$ block solutions so the remaining $d$ linear equations can be satisfied by a nonzero vector of block coefficients.

The only randomized step is finding a quadratic non-residue modulo $p$. The constructive algorithm matters: Chevalley--Warning by itself gives existence of a nonzero solution, but the quantum hiding procedure needs the actual vector $j$.

| Feature | Extraspecial HSP | Nil-2 HSP |
|---|---|---|
| Derived subgroup | one-dimensional $G'\cong \mathbb Z_p$ | $d$-dimensional $G'\cong \mathbb Z_p^d$ after reduction |
| Copy count in phase cancellation | four phase-labelled copies | polynomial in $d$, with $n=(d+1)^2(d+2)/2$ in the construction |
| Equations for automorphism phases | two scalar equations in the $j_i$ | $d$ quadratic and $d$ linear equations |
| Witness method | quadratic-residue calculation | constructive Chevalley--Warning-style search |

## Quantum tricks

- [[Automorphism-Action Reduction to Abelian HSP]] — the inherited engine from the extraspecial-group paper.
- [[Chevalley-Warning Search for Nil-2 HSP Actions]] — the reusable part of the witness-construction proof.
- [[HSP as Algorithmic Template (Abelian)]] — used after reducing to $HG' \cap \overline G$ in an abelian representative group.
- Nonabelian QFS Reconstruction Checklist — places this algorithm among HSP methods that bypass direct nonabelian Fourier-sampling reconstruction.

## Connections

- Directly extends [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes|Ivanyos-Sanselme-Santha (2007)]]. Extraspecial groups have one-dimensional $G'$; nil-2 groups require $d$ central coordinates.
- Adjacent to [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes|Childs-van Dam (2010)]], which surveys HSP successes and barriers.
- Adjacent to [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill (2004)]], since this paper supplies an efficient measurement/reconstruction method for a special nonabelian family.
- Related to [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]], another positive result for structured nonabelian HSP.
- Review context: [[The Hidden Subgroup Problem Review and Open Problems (Lomont 2004) — Paper Notes|Lomont (2004)]].

## Why it matters

This is a strong example of an HSP algorithm where group-theoretic preprocessing and low-degree algebra substitute for a hard nonabelian measurement problem. It also records a reusable pattern: if automorphisms convert hidden-subgroup membership into polynomial phase constraints over a finite field, finite-field existence theorems can become algorithmic subroutines.

## Caveats

- The reduction relies on constant nilpotency class and on polycyclic-presentation algorithms.
- The final quantum construction is tailored to nil-2 exponent-$p$ structure.
- The number of copies scales polynomially in $d = \dim G'$, not constant as in the extraspecial case.
- The result is not a route to general nonabelian HSP or symmetric-group HSP.
