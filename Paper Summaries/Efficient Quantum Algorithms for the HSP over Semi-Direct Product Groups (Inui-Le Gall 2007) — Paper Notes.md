# Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes

> **Source:** Yoshifumi Inui and François Le Gall, *Efficient quantum algorithms for the hidden subgroup problem over a class of semi-direct product groups*, Quantum Information and Computation 7(5/6):559–570 (2007); arXiv:quant-ph/0412033.
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0412033)
> **Tags:** #hidden-subgroup-problem #nonabelian #semidirect-product #black-box-groups #abelian-reduction

---

## Metadata

- **Authors:** Yoshifumi Inui; François Le Gall
- **Venue:** Quantum Information and Computation 7(5/6):559–570, 2007
- **arXiv:** quant-ph/0412033
- **Algorithm Zoo entry:** #53
- **Group families:** $\mathbb Z_{p^r}\rtimes \mathbb Z_q$ for classification; efficient HSP algorithm for $P_{p,r}=\mathbb Z_{p^r}\rtimes\mathbb Z_p$ with action $yx=x^{p^{r-1}+1}y$; extension to $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ under a specified input form.
- **Input models:** black-box groups with not necessarily unique encoding for $P_{p,r}$; unique encoding plus separated generators for the $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ extension.

---

## The computational problem

**Hidden subgroup problem (HSP):** given a group $G$ by generators and oracle access to a function $f:G\to X$ that is constant and distinct on left cosets of an unknown $H\leq G$, output generators for $H$ in time polynomial in $\log |G|$.

This paper studies semidirect products of cyclic groups

$$
\mathbb Z_n\rtimes_\phi \mathbb Z_q,
$$

with product

$$
(a_1,b_1)(a_2,b_2)=(a_1+\phi(b_1)(a_2), b_1+b_2),
$$

and in particular the prime-power case $n=p^r$ with $p,q$ prime.

---

## What the paper does

The paper has two main parts.

1. **Classifies** the non-isomorphic semidirect products $\mathbb Z_{p^r}\rtimes\mathbb Z_q$ for prime $p,q$ into five classes.
2. Gives efficient quantum HSP algorithms for one of the nonabelian classes,

$$
P_{p,r}=\langle x,y\mid x^{p^r}=y^p=e,\; yx=x^{p^{r-1}+1}y\rangle,
$$

for $p$ prime and $r\geq2$ excluding $p=r=2$.

The $P_{p,r}$ algorithm is notable because it works even when the group is a black-box group with nonunique encodings. It combines a subgroup classification with normal-HSP machinery and a carefully constructed abelian subgroup in which the only nonnormal subgroups can be found by abelian Fourier sampling.

The nonunique-encoding tolerance is specific to the $P_{p,r}$ black-box treatment, where the algorithm can recover suitable orders, commutation data, and an abelian window from black-box generators. The later $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ extension uses an explicit abelianization bijection and therefore assumes unique encoding plus separated generators for the two factors.

The paper then extends the method to

$$
\mathbb Z_{p^r}^m\rtimes\mathbb Z_p
$$

when the input gives generators for the left abelian part and a generator for the right $\mathbb Z_p$ part.

---

## Algorithm / construction

### 1. Classify $\mathbb Z_{p^r}\rtimes\mathbb Z_q$

A homomorphism $\phi:\mathbb Z_q\to\operatorname{Aut}(\mathbb Z_{p^r})$ is specified by

$$
\alpha=\phi(1)(1)\in\mathbb Z_{p^r}^*,\qquad \alpha^q\equiv1\pmod{p^r}.
$$

The paper shows that nontrivial possibilities occur only in three cases:

- $q\mid p-1$;
- $r>1$ and $q=p\neq2$, with $\alpha=t p^{r-1}+1$ for $0<t<p$;
- $r>1$ and $q=p=2$, with the familiar order-two automorphisms of $\mathbb Z_{2^r}$.

Up to isomorphism this yields five classes:

- q-hedral groups $\mathbb Z_{p^r}\rtimes\mathbb Z_q$ with $q\mid p-1$;
- dihedral groups $D_{2^r}$;
- quasi-dihedral groups $QD_{2^r}$;
- $P_{p,r}$ with action $yx=x^{p^{r-1}+1}y$;
- direct products $\mathbb Z_{p^r}\times\mathbb Z_q$.

### 2. Classify subgroups of $P_{p,r}$

For

$$
P_{p,r}=\langle x,y\mid x^{p^r}=y^p=e,\; yx=x^{p^{r-1}+1}y\rangle,
$$

Proposition 7 lists all subgroups:

- $\langle x^{p^i}\rangle$ for $0\leq i\leq r$;
- $\langle x^{p^i},y\rangle$ for $0\leq i\leq r$;
- $\langle x^{t p^j}y\rangle$ with $0\leq j<r$ and $1\leq t<p$.

Proposition 8 says all subgroups are abelian except $P_{p,r}$ itself, and the only nonnormal subgroups are the $p$ subgroups

$$
\langle x^{t p^{r-1}}y\rangle,
\qquad 0\leq t<p.
$$

This reduces the HSP to two cases.

### 3. If the hidden subgroup is normal

Run the normal-HSP algorithm of [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha]]. This handles normal hidden subgroups in the black-box setting used by the paper.

### 4. If the hidden subgroup is nonnormal

Then

$$
H=\langle x^{t p^{r-1}}y\rangle
$$

for some $t\in\mathbb Z_p$.

In a black-box presentation the named elements $x,y$ may not be directly available. The algorithm starts from two generators $X=x^ay^b$ and $Y=x^{a'}y^{b'}$ such that $X$ has order $p^r$ and $X,Y$ do not commute, which implies

$$
ab'\not\equiv a'b\pmod p.
$$

It then finds an integer $l$ satisfying $(X^p)^l=Y^p$ by reducing to an abelian HSP over $\mathbb Z_{p^{r-1}}\times\mathbb Z_{p^{r-1}}$. From this $l$ it constructs

$$
Y'=X^{-l}Y=x^{\alpha p^{r-1}}y^\beta,
\qquad \beta\not\equiv0\pmod p
$$

for odd $p$. Then

$$
\langle X^{p^{r-1}},Y'\rangle=\langle x^{p^{r-1}},y\rangle\cong\mathbb Z_p\times\mathbb Z_p,
$$

an abelian subgroup containing all nonnormal candidates. Abelian Fourier sampling in this subgroup recovers the hidden subgroup. The paper gives the analogous $p=2$ construction using $\langle x^{p^{r-2}},y\rangle\cong\mathbb Z_{p^2}\times\mathbb Z_p$.

### 5. Extension to $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$

The group is generated by $x_1,\dots,x_m,y$ with

$$
yx_i=x_i^{p^{r-1}+1}y.
$$

For the special input form where the $\mathbb Z_{p^r}^m$ part and the $\mathbb Z_p$ generator are separated, the paper defines a bijection

$$
\pi:g_1^{a_1}\cdots g_m^{a_m}y^b\mapsto z_1^{a_1}\cdots z_m^{a_m}z_{m+1}^b
$$

from the semidirect product to the abelian group

$$
G'=\mathbb Z_{p^r}^m\times\mathbb Z_p.
$$

It proves that $\pi(H)$ is a subgroup and that $f\circ\pi^{-1}$ hides $\pi(H)$ in $G'$. The HSP then reduces to the abelian HSP.

---

## Key results

- **Theorem 6:** the groups $\mathbb Z_{p^r}\rtimes\mathbb Z_q$ for prime $p,q$ and $r\geq1$ fall into five non-isomorphic classes: q-hedral, dihedral, quasi-dihedral, $P_{p,r}$, and direct product.
- **Theorem 9:** for $P_{p,r}$ input as a black-box group with not necessarily unique encoding, there is a quantum polynomial-time algorithm for HSP.
- **Theorem 11:** for $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ given with unique encoding and separated generators for the two factors, there is a quantum polynomial-time HSP algorithm.

The algorithmic theorem is for the $P_{p,r}$ class and its explicitly structured $m$-factor extension. The classification theorem also names dihedral and quasi-dihedral groups, but this paper does not solve those HSPs.

---

## Comparison

- **[[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha (2001)]]:** supplies the black-box normal-HSP and abelian membership machinery reused here. Inui--Le Gall adds a group-specific treatment of nonnormal subgroups in $P_{p,r}$.
- **[[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes|Gavinsky (2004)]]:** uses normalizer intersections to reduce near-Hamiltonian HSPs to normal-HSP calls. Inui--Le Gall uses an explicit subgroup classification for $P_{p,r}$.
- **[[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]]:** treats broad $A\rtimes\mathbb Z_p$ families via PGM and matrix-sum problems. Inui--Le Gall obtains a black-box algorithm for $P_{p,r}$ without requiring explicit PGM implementation.
- **[[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes|Chi--Kim--Lee (2006)]]:** builds on the $\mathbb Z_{p^r}\rtimes\mathbb Z_p$ result by splitting a restricted $\mathbb Z_N\rtimes\mathbb Z_p$ instance into an abelian factor and an Inui--Le Gall factor.

---

## Limits / caveats

- The result does not solve the dihedral or quasi-dihedral HSP classes. The paper explicitly keeps them among the main open semidirect-product cases.
- The $P_{p,r}$ theorem excludes the degenerate case $p=r=2$.
- The $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ extension needs more input structure than a fully arbitrary black-box group: the algorithm must isolate generators of the left abelian part and a generator of the right $\mathbb Z_p$ part.
- For $P_{p,r}$, the proof relies on finding suitable noncommuting generators and using black-box group operations coherently; it is not a generic recipe for all semidirect products.

---

## Reusable ideas

1. **[[Classify Subgroups Before Measuring in Semidirect-Product HSP]]:** A full subgroup classification can reduce a nonabelian HSP to a normal case plus a small explicit family of nonnormal candidates.
2. **[[Build an Abelian Window Around Nonnormal HSP Candidates]]:** When all hard candidates lie in a small abelian subgroup, construct that subgroup from black-box generators and solve the remaining task by abelian Fourier sampling.
3. **[[Abelianization Bijection for Structured Semidirect HSP]]:** If a bijection to an abelian group maps hidden cosets to hidden cosets and is implementable, the HSP can be solved by running the abelian HSP on the image.

---

## References within this paper

- Ajtai--Dwork (1997) and Regev lattice cryptography papers — motivation through the dihedral HSP and lattice problems.
- Babai--Szemerédi and Babai et al. — black-box groups and random generation.
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]] — PGM method for $A\rtimes\mathbb Z_p$.
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha (2001)]] — normal-HSP and black-box group algorithms.
- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes|Chi--Kim--Lee (2006)]] — later extension based on this family.
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] and [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — dihedral context.
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — black-box solvable group algorithms.

---

## Cross-links

### Paper notes

- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]]
- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]]
- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]

### Trick cards

- [[Classify Subgroups Before Measuring in Semidirect-Product HSP]]
- [[Build an Abelian Window Around Nonnormal HSP Candidates]]
- [[Abelianization Bijection for Structured Semidirect HSP]]
