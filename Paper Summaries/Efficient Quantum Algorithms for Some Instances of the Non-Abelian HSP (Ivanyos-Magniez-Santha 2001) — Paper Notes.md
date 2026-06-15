# Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes

> **Source:** Gábor Ivanyos, Frédéric Magniez, and Miklos Santha, *Efficient quantum algorithms for some instances of the non-Abelian hidden subgroup problem*, Proc. 13th ACM Symposium on Parallel Algorithms and Architectures, pp. 263–270 (2001); later International Journal of Foundations of Computer Science 14(5):723–740 (2003); arXiv:quant-ph/0102014.
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0102014)
> **Tags:** #hidden-subgroup-problem #nonabelian #black-box-groups #normal-hsp #commutator-subgroup #abelian-reduction

---

## Metadata

- **Authors:** Gábor Ivanyos; Frédéric Magniez; Miklos Santha
- **Conference:** SPAA 2001, pp. 263–270
- **Journal version:** International Journal of Foundations of Computer Science 14(5):723–740, 2003
- **arXiv:** quant-ph/0102014
- **Algorithm Zoo entry:** #55
- **Input model:** finite black-box groups, often with unique encoding for the full HSP results; nonunique encoding allowed in several normal-subgroup and quotient computations.
- **Core tools:** Shor order finding, abelian HSP, constructive membership in abelian subgroups, Beals--Babai black-box group algorithms, normal-HSP reconstruction.

---

## The computational problem

**Hidden subgroup problem (HSP):** given a finite group $G$ by generators and oracle access to $f$ that is constant on each left coset of an unknown subgroup $H\leq G$ and distinct on distinct cosets, find generators for $H$.

The paper targets nonabelian HSP instances that can be reduced to abelian or normal-subgroup tasks in black-box groups. It solves:

- hidden **normal** subgroups in solvable black-box groups and permutation groups;
- HSP in groups with small commutator subgroup $G'$;
- HSP in groups with an elementary abelian normal $2$-subgroup $N$ of small index, or with cyclic quotient $G/N$.

---

## What the paper does

This is an early bridge between quantum HSP methods and computational group theory. Instead of assuming an efficient nonabelian Fourier transform on $G$, it imports Beals--Babai black-box group algorithms and shows that the remaining “abelian obstacles” can be handled quantumly using Shor-type routines and the abelian HSP.

The paper's recurring pattern is:

1. use normal or abelian structure to find part of $H$;
2. work in a quotient or a small extension;
3. lift generators back to $G$;
4. prove that the lifted subgroup $H_1\leq H$ has both the same intersection with a normal subgroup and the same image in the quotient, hence $H_1=H$.

This pattern is later reused by papers such as [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes|Gavinsky (2004)]] and [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes|Inui--Le Gall (2007)]].

---

## Algorithm / construction

### 1. Quantum implementation of Beals--Babai tasks

Beals--Babai algorithms solve group-structure tasks in black-box groups, assuming access to order computations and constructive membership tests in elementary abelian subgroups. Ivanyos--Magniez--Santha note that:

- Shor's algorithm supplies order finding and discrete logarithms;
- constructive membership in abelian subgroups can be reduced to an abelian HSP.

For commuting $h_1,\dots,h_r$ and a target $g$, compute the orders $s_1,\dots,s_r,s$ and define

$$
\varphi(\alpha_1,\dots,\alpha_r,\alpha)=h_1^{\alpha_1}\cdots h_r^{\alpha_r}g^{-\alpha}
$$

on

$$
\mathbb Z_{s_1}\times\cdots\times\mathbb Z_{s_r}\times\mathbb Z_s.
$$

The kernel is an abelian HSP instance. It contains an element whose last coordinate is coprime to $s$ iff $g$ lies in $\langle h_1,\dots,h_r\rangle$, and such a kernel element gives an expression for $g$.

### 2. Hidden normal subgroup reconstruction

If $N\triangleleft G$ is hidden, then the hiding function gives a secondary encoding of $G/N$. The paper shows that the structural tasks needed in $G/N$ can be performed quantumly. A presentation of $G/N$ is then used to lift relators and original generators back to $G$ and generate the normal closure equal to $N$.

Result: hidden normal subgroups of solvable black-box groups and permutation groups can be found in quantum polynomial time, without assuming an efficient nonabelian QFT over the whole group.

### 3. Groups with small commutator subgroup

Let $G'$ be the commutator subgroup. The theorem solves HSP in time polynomial in the input size plus $|G'|$.

Algorithm outline:

1. Enumerate $G'$ and compute $H\cap G'$ by checking the hiding function.
2. Define

$$
F(x)=\{f(xg):g\in G'\}.
$$

This hides $HG'$, which is normal because $G/G'$ is abelian.
3. Use the normal-HSP algorithm to find generators for $HG'$.
4. For each generator coset $xG'$ of $HG'/G'$, enumerate $xG'$ and pick an element in $H$.
5. Generate $H_1$ from those lifted elements and $H\cap G'$.

The proof uses

$$
H_1\cap G'=H\cap G',\qquad H_1G'=HG'
$$

to conclude $H_1=H$.

A direct corollary is an efficient algorithm for extraspecial $p$-groups in time polynomial in input size plus $p$, since $|G'|=p$.

### 4. Elementary abelian normal 2-subgroup

Let $N\triangleleft G$ be elementary abelian of exponent $2$, given by generators. The goal is to lift enough quotient generators from $HN/N$ back into $H$.

First compute $H\cap N$ by the abelian HSP. Then choose a set $V\subseteq G$ that contains a generating set for every subgroup of $G/N$:

- if $G/N$ is cyclic, build $V$ from generators of Sylow subgroups of $G/N$;
- if $|G/N|$ is small, take coset representatives.

For each $z\in V\setminus\{1\}$, define a function on the abelian group $\mathbb Z_2\times N$:

$$
F(0,x)=f(x),\qquad F(1,x)=f(xz).
$$

This hides either

$$
\{0\}\times(H\cap N)
$$

or

$$
(\{0\}\times(H\cap N))\cup(\{1\}\times u(H\cap N))
$$

for some $u\in zH\cap N$. Because $N$ is elementary abelian of order a power of $2$, this is a subgroup of $\mathbb Z_2\times N$. Abelian HSP recovers a generator $(1,u)$ when $zN$ meets $HN/N$, yielding $u^{-1}z\in H$.

---

## Key results

Here $\nu(G)$ is the Beals--Babai black-box group parameter controlling the degree of the nonabelian composition factors that appear in the classical permutation-representation subroutines. It is part of the runtime, not a hidden constant.

- **Theorem 6:** for black-box groups with unique encoding, Beals--Babai structural tasks from Corollary 5 can be implemented in quantum polynomial time in the input size plus $\nu(G)$.
- **Theorem 7:** if a normal subgroup $N$ is hidden in a black-box group with nonunique encoding, the same structural tasks can be done for $G/N$ in time polynomial in the input size plus $\nu(G/N)$.
- **Theorem 8:** hidden normal subgroups of solvable black-box groups and permutation groups can be found in quantum polynomial time.
- **Theorem 11:** if $G$ has unique encoding, HSP in $G$ is solvable in time polynomial in the input size plus $|G'|$.
- **Corollary 12:** HSP in extraspecial $p$-groups is solvable in time polynomial in input size plus $p$.
- **Theorem 13:** if $N\triangleleft G$ is an elementary abelian normal $2$-subgroup given by generators, HSP is solvable in time polynomial in input size plus $|G/N|$; if $G/N$ is cyclic, it is solvable in polynomial time.

---

## Comparison

- **[[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev]] / [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]]:** supply the abelian HSP and order-finding engines. IMS uses them as subroutines inside black-box group algorithms.
- **Hallgren--Russell--Ta-Shma normal subgroup reconstruction:** required efficient QFT over the group; IMS gives normal-HSP results in broad black-box settings without that assumption for solvable and permutation groups.
- **[[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]]:** provides solvable-group order, membership, and uniform-state tools. IMS uses related black-box methods and extends normal-HSP applications.
- **[[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes|Gavinsky (2004)]]:** uses normal-HSP subroutines as a building block; IMS is one source of those subroutines in black-box group settings.
- **[[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes|Inui--Le Gall (2007)]]:** explicitly cites IMS as the prior black-box HSP framework and uses its normal-HSP routine for $P_{p,r}$.

---

## Limits / caveats

- Several full HSP results require unique encoding. Nonunique encoding is handled for important quotient/normal-subgroup steps but not for every theorem.
- The small-commutator theorem is efficient only when $|G'|$ is polynomially bounded in the input size.
- The elementary abelian normal-subgroup theorem uses exponent $2$ in the lifting test; the union of two cosets becomes a subgroup of $\mathbb Z_2\times N$ in a way that would fail for many other extensions.
- The method does not solve the generic symmetric-group HSP or the general dihedral HSP.
- The paper relies on classical black-box group algorithms whose runtime depends on $\nu(G)$, a parameter controlling the permutation-representation degree of nonabelian composition factors.

---

## Reusable ideas

1. **[[Constructive Membership via Abelian HSP Kernel]]:** Express membership in an abelian subgroup as the kernel of a homomorphism from a product of cyclic groups, then solve that kernel with the abelian HSP.
2. **[[Lift Hidden Subgroups from Normal Quotients]]:** Find $H\cap N$ and $HN/N$, then lift quotient generators into $H$; if both the intersection and quotient image match, the lifted subgroup is $H$.
3. **[[Use Small Commutator Subgroup to Abelianize HSP]]:** Enumerate $G'$ so that the quotient $G/G'$ is abelian, find $HG'$ as a hidden normal subgroup, then lift representatives from cosets.
4. **[[Two-Coset Abelian Test for Elementary 2-Extensions]]:** For $N$ elementary abelian, test whether $zN$ contains a hidden-subgroup element by hiding a subgroup of $\mathbb Z_2\times N$.

---

## References within this paper

- Beals--Babai and Babai--Szemerédi — black-box group algorithms and membership obstacles.
- Cheung--Mosca — decomposition of finite abelian black-box groups.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — order finding, discrete logarithms.
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — abelian stabilizer/HSP.
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994/1997)]] — abelian HSP precursor.
- Hallgren--Russell--Ta-Shma — normal subgroup reconstruction.
- Grigni--Schulman--Vazirani--Vazirani — almost-abelian nonabelian HSP.
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — solvable black-box group algorithms.
- Rötteler--Beth — wreath product HSP.

---

## Cross-links

### Paper notes

- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]]
- [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]

### Trick cards

- [[Constructive Membership via Abelian HSP Kernel]]
- [[Lift Hidden Subgroups from Normal Quotients]]
- [[Use Small Commutator Subgroup to Abelianize HSP]]
- [[Two-Coset Abelian Test for Elementary 2-Extensions]]
