# An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes

> **Source:** Gábor Ivanyos, Luc Sanselme, and Miklos Santha, *An efficient quantum algorithm for the hidden subgroup problem in extraspecial groups*, STACS 2007; arXiv:quant-ph/0701235
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0701235)
> **Tags:** #hidden-subgroup-problem #nonabelian #extraspecial-groups

---

## TL;DR

Ivanyos, Sanselme, and Santha give a polynomial-time quantum algorithm for the hidden subgroup problem (HSP) in all extraspecial groups. The main move is not to perform a complete nonabelian Fourier-transform reconstruction. Instead, the algorithm uses automorphisms of extraspecial groups to build a quantum hiding procedure for $HG'$ over the abelian quotient-like model $G/G'$, then applies the standard abelian HSP algorithm. For large exponent-$p$ groups, four phase-labelled coset states are combined so that automorphism-induced phases cancel.

## Metadata

- **Authors:** Gábor Ivanyos, Luc Sanselme, Miklos Santha
- **Year:** 2007
- **Venue:** STACS 2007
- **arXiv:** [quant-ph/0701235](http://arxiv.org/abs/quant-ph/0701235)
- **Source text read:** `/tmp/hsp_zoo_all/zoo56_quant-ph_0701235.txt`
- **Algorithm Zoo:** #56 ✓
- **Status:** Paper note created from full extracted text.

## Problem setting

Given a finite group $G$, an oracle $f$ hides a subgroup $H \leq G$ if $f$ is constant on left cosets of $H$ and distinct on different cosets. The task is to output generators for $H$ in time polynomial in $\log |G|$.

Extraspecial groups are $p$-groups with

$$G' = Z(G) = \Phi(G),$$

and center cyclic of order $p$. Hence $|G| = p^{2k+1}$, and elements can be encoded using generators

$$x_1,y_1,\ldots,x_k,y_k,z,$$

with a unique normal form

$$x_1^{i_1} y_1^{i'_1}\cdots x_k^{i_k}y_k^{i'_k}z^\ell.$$

The nonabelian part sits entirely in the central subgroup $G' = \langle z\rangle$.

## Main result

Theorem 2 states that for any extraspecial $p$-group $G$ and oracle $f$ hiding $H$, there is an efficient quantum procedure that finds $H$.

The proof splits into:

- constant exponent groups, already covered by prior bounded-$|G'|$ methods but rederived in the paper;
- exponent-$p$ groups with large $p$, the main technical case;
- exponent-$p^2$ groups, reduced to the exponent-$p$ case by restricting to an exponent-$p$ subgroup when $G' \cap H = \{1\}$.

## Reduction structure

The reductions first show that it is enough to find $HG'$.

1. If $G' \subseteq H$, then $H = HG'$.
2. If $G' \cap H = \{1\}$, then $H$ is abelian and $f|_{HG'}$ hides $H$ in the abelian subgroup $HG'$.
3. Define $\overline G$ as the set of representatives obtained by deleting the central $z^\ell$ coordinate, with group law inherited from $G/G'$. Then $\overline G \cong \mathbb Z_p^{2k}$.
4. Finding $HG' \cap \overline G$ by abelian HSP gives $HG'$, and then gives $H$.

The missing ingredient is therefore a quantum hiding procedure for $HG'$ on the abelian representative group $\overline G$.

Operationally, because $G'=\langle z\rangle$ has prime order, the algorithm first separates the cases $G'\subseteq H$ and $G'\cap H=\{1\}$. Once $HG'$ is known in the second case, restricting the oracle to the abelian subgroup $HG'$ and running abelian HSP recovers the missing complement $H$.

## Core algorithmic idea

For exponent-$p$ extraspecial groups, the algorithm creates phase-labelled coset states

$$|aHG'_u\rangle,$$

where the central register has been Fourier-transformed over $G' \cong \mathbb Z_p$. Right multiplication by the center gives a phase:

$$|aHG'_u \cdot z\rangle = \omega^u |aHG'_u\rangle.$$

Automorphisms $\phi_j$ are defined on generators by

$$x_i \mapsto x_i^j,\quad y_i \mapsto y_i^j,\quad z \mapsto z^{j^2}.$$

For $h \in H$, $\phi_j(h)$ acts on $|aHG'_u\rangle$ by a phase of the form

$$\omega^{u(j-j^2)\ell},$$

while central elements act with phase $\omega^{u j^2 t}$.

After the Fourier labels $u_1,\ldots,u_4$ are known, the algorithm chooses $j_1,\ldots,j_4$ so both phase sums vanish:

$$\sum_{i=1}^4 u_i(j_i-j_i^2)=0,\qquad \sum_{i=1}^4 u_i j_i^2=0.$$

When this holds, the combined state is unchanged by all $g \in HG'$, but states for different cosets of $HG'$ are orthogonal. This is exactly a quantum hiding procedure for $HG'$.

## Witness generation

For random $u \in \mathbb Z_p^4$, the system above has a solution with probability at least $(p-9)/(2p)$. The proof fixes $j_3=1$, $j_4=-1$, reduces to a quadratic equation in $j_1$, and uses quadratic-residue testing plus modular square roots to find a witness when one exists. Thus the expected number of trials is constant for large $p$.

The displayed lower bound also flags that small primes need separate handling; the paper treats the bounded-size exceptional cases by earlier small-commutator methods.

## Quantum tricks

- [[Automorphism-Action Reduction to Abelian HSP]] — use automorphism-controlled right actions to turn a nonabelian HSP instance into an abelian HSP over $G/G'$-style representatives.
- [[Chevalley-Warning Search for Nil-2 HSP Actions]] — later nil-2 generalisation of the phase-cancellation witness search.
- [[HSP as Algorithmic Template (Abelian)]] — final reconstruction step once the hidden object is moved to an abelian group.
- Nonabelian QFS Reconstruction Checklist — explains why this paper avoids relying on weak/strong nonabelian Fourier sampling as the extraction method.

## Connections

- Generalises the Heisenberg-group work cited in the paper, while avoiding full representation-theoretic Fourier sampling.
- Direct predecessor of [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) — Paper Notes|Ivanyos-Sanselme-Santha (2008)]]. The nil-2 paper keeps the automorphism-action idea but replaces the four-variable witness with a larger system of quadratic and linear equations.
- Adjacent to [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]], which solves other nonabelian HSP families using optimal measurements.
- Adjacent to [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill (2004)]], which shows that query access is not the bottleneck for finite-group HSP.
- The review backdrop is [[The Hidden Subgroup Problem Review and Open Problems (Lomont 2004) — Paper Notes|Lomont (2004)]].

## Why it matters

The paper is a clean example of a nonabelian HSP success that does not come from simply applying a nonabelian QFT and measuring representation labels. It uses group structure to manufacture an abelian HSP instance. This is useful as a design pattern: if the noncommutativity is central and controllable by automorphisms, phase cancellation can expose the quotient data needed for subgroup recovery.

## Caveats

- The method depends on the very special structure $G' = Z(G)$ and on explicit automorphisms with predictable action on central phases.
- The exponent-$p^2$ case is handled by reduction, not by a separate phase-cancellation construction.
- The approach does not address symmetric-group HSP or graph isomorphism barriers.
