# Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes

> **Source:** Dong Pyo Chi, Jeong San Kim, and Soojoon Lee, *Notes on the hidden subgroup problem on some semi-direct product groups*, Phys. Lett. A 359(2):114–116 (2006); arXiv:quant-ph/0604172
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0604172)
> **Tags:** #hidden-subgroup-problem #nonabelian #semidirect-product #group-decomposition

---

## The computational problem

**Hidden subgroup problem (HSP):** Given oracle access to a function $f:G\to S$ satisfying

$$
f(a)=f(b) \quad \Longleftrightarrow \quad aH=bH,
$$

find the hidden subgroup $H\leq G$.

This paper studies

$$
G=\mathbb{Z}_N\rtimes_\phi \mathbb{Z}_p,
$$

where $p$ is prime, $N=\prod_{j=1}^n p_j^{r_j}$, and the restriction is that $p\nmid p_j-1$ for every prime factor $p_j$ of $N$. The nontrivial action $\phi$ has order $p$, so $p\mid \varphi(N)$; under the paper's restriction this forces one factor $p_k=p$ with $r_k\geq 2$.

---

## What the paper does

This is a short structural note rather than a new measurement method. The useful observation is that the restriction $p\nmid p_j-1$ makes the semidirect product split into an abelian cyclic factor times a known smaller semidirect-product instance:

$$
\mathbb{Z}_N\rtimes_\phi \mathbb{Z}_p \cong \mathbb{Z}_{N/p^r}\times (\mathbb{Z}_{p^r}\rtimes_\psi \mathbb{Z}_p).
$$

Then any hidden subgroup splits across the two relatively-prime factors. The first part is an abelian HSP; the second is the Inui--Le Gall family $\mathbb{Z}_{p^r}\rtimes \mathbb{Z}_p$. My assessment: technically clean, but incremental. It packages an existing semidirect-product algorithm inside a CRT/group-factorisation argument.

---

## The algorithm / construction

### 1. Describe the semidirect product

For $\phi:\mathbb{Z}_p\to\operatorname{Aut}(\mathbb{Z}_N)$,

$$
(a_1,b_1)(a_2,b_2)=(a_1+\phi(b_1)(a_2), b_1+b_2).
$$

Writing $x=(1,0)$ and $y=(0,1)$ gives the relation

$$
y^b x^a = x^{a\phi(1)(1)^b}y^b.
$$

A nontrivial semidirect product requires $\phi(1)(1)\in \mathbb{Z}_N^*$ to have order $p$.

### 2. Split the action prime-power by prime-power

Use the Chinese remainder theorem:

$$
\mathbb{Z}_N \cong \prod_{j=1}^n \mathbb{Z}_{p_j^{r_j}}.
$$

Lemma 1 says automorphisms preserve each prime-power component. For two distinct primes $p,q$, an automorphism of

$$
\mathbb{Z}_{q^s}\times \mathbb{Z}_{p^r}
$$

cannot mix the two components.

Lemma 2 then uses $p\nmid q-1$ to show the $\mathbb{Z}_p$ action is trivial on $\mathbb{Z}_{q^s}$. So only the $p$-power component can carry the nontrivial action. This yields

$$
\mathbb{Z}_N\rtimes_\phi \mathbb{Z}_p
\cong
\mathbb{Z}_{N/p^r}\times (\mathbb{Z}_{p^r}\rtimes_\psi \mathbb{Z}_p).
$$

### 3. Split the hidden subgroup

Lemma 3: if $|G_0|$ and $|G_1|$ are coprime, then every subgroup of $G_0\times G_1$ is a product $H_0\times H_1$.

Here,

$$
\gcd\bigl(|\mathbb{Z}_{N/p^r}|, |\mathbb{Z}_{p^r}\rtimes \mathbb{Z}_p|\bigr)=1,
$$

so the hidden subgroup decomposes as $H=H_1\times H_2$.

### 4. Solve the two HSP instances

- Solve the abelian HSP over $\mathbb{Z}_{N/p^r}$ using the standard QFT method, ultimately rooted in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]]/[[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev]].
- Solve the $\mathbb{Z}_{p^r}\rtimes \mathbb{Z}_p$ component using the Inui--Le Gall subgroup-classification algorithm.
- Combine the subgroup generators through the direct product isomorphism.

### Appendix: $\mathbb{Z}_{2p^r}\rtimes \mathbb{Z}_p$

The appendix extends the same subgroup-classification method to $\mathbb{Z}_{2p^r}\rtimes \mathbb{Z}_p$ for odd $p$. It shows that all nontrivial actions are isomorphic, with presentation

$$
\langle x,y\mid x^{2p^r}=y^p=e,\; yx=x^{2p^{r-1}+1}y\rangle.
$$

Every subgroup is either

$$
\langle x^{2^t p^s}\rangle
$$

or

$$
\langle x^{2^t p^s}, x^{h2^t p^{s-1}}y\rangle,
$$

where $0\leq t\leq 1$, $0\leq s\leq r$, and $0\leq h\leq p-1$.

Given $H\cap\langle x\rangle=\langle x^{2^t p^s}\rangle$, the algorithm prepares

$$
\frac{1}{p}\sum_{a,b\in\mathbb{Z}_p}|a\rangle|b\rangle|f(x^{a2^t p^{s-1}}y^b)\rangle,
$$

measures the oracle register, applies $F_p\otimes F_p$, and obtains $(\tilde c,\tilde d)$. If $\tilde c\neq 0$, it outputs

$$
\tilde h=-\tilde c^{p-2}\tilde d \pmod p.
$$

For a noncyclic subgroup this returns the correct $h$ with probability $1-1/p$ per good run; for the cyclic case the candidate $\tilde h$ is uniform, so repeated runs distinguish the two cases.

---

## Key results

**Lemma 2.** Let $p$ and $q$ be distinct primes with $p\nmid q-1$. Then

$$
(\mathbb{Z}_{q^s}\times \mathbb{Z}_{p^r})\rtimes_\phi \mathbb{Z}_p
=\mathbb{Z}_{q^s}\times (\mathbb{Z}_{p^r}\rtimes_\psi \mathbb{Z}_p)
$$

for some $\psi:\mathbb{Z}_p\to\operatorname{Aut}(\mathbb{Z}_{p^r})$.

**Lemma 3.** If $|G_0|$ and $|G_1|$ are coprime and $H\leq G_0\times G_1$, then $H=H_0\times H_1$ for subgroups $H_i\leq G_i$.

**Theorem 1.** There is an efficient quantum algorithm for the HSP on $\mathbb{Z}_N\rtimes_\phi\mathbb{Z}_p$ when $N=\prod_j p_j^{r_j}$, the action has the paper's nontrivial order-$p$ form, and $p\nmid p_j-1$ for every prime factor $p_j$ outside the $p$-power component.

For the appendix case $\mathbb{Z}_{2p^r}\rtimes\mathbb{Z}_p$, the HSP algorithm runs in time

$$
O((r\log p)^2)
$$

after the abelian-HSP step used to find $H\cap\langle x\rangle$.

---

## Comparison with prior work

| Paper | Group family | Technique | Status |
|---|---|---|---|
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] / [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] | Abelian groups | Fourier sampling | Efficient |
| [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes|Roetteler--Beth (1998)]] | $\mathbb{Z}_2^n\wr\mathbb{Z}_2$ | custom nonabelian Fourier sampling + linear algebra | Efficient |
| Inui--Le Gall (2007) | $\mathbb{Z}_{p^r}\rtimes\mathbb{Z}_p$ | subgroup classification + 2D Fourier test | Efficient |
| [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]] | $A\rtimes\mathbb{Z}_p$ | PGM + matrix sum problem | Broader measurement framework |
| [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes|Cosme--Portugal (2007)]] | $\mathbb{Z}_{p^r}\rtimes\mathbb{Z}_{p^2}$ and restricted $\mathbb{Z}_N\rtimes\mathbb{Z}_{p^2}$ | classification + abelian restrictions + slope tests | Efficient for narrow families |
| This paper | $\mathbb{Z}_N\rtimes\mathbb{Z}_p$ with $p\nmid p_j-1$ | factor splitting + Inui--Le Gall | Efficient, but narrow |

---

## Limits / caveats

- The restriction $p\nmid p_j-1$ is doing most of the work. It prevents nontrivial $\mathbb{Z}_p$ action on all prime-power components except the $p$-power component.
- This does not address the hard dihedral case $\mathbb{Z}_N\rtimes\mathbb{Z}_2$ when the nontrivial action is inversion; see [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg]] and [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev]].
- The paper does not give a new general nonabelian HSP measurement. It reduces to existing efficient instances.
- The appendix's $O((r\log p)^2)$ claim is for the classified $\mathbb{Z}_{2p^r}\rtimes\mathbb{Z}_p$ family, not for arbitrary semidirect products.

---

## Reusable ideas

1. **[[Coprime-Factor Splitting for Semidirect-Product HSP]]:** When a semidirect-product action is forced to be trivial on coprime direct factors, the HSP splits into independent HSPs on smaller groups.
2. **[[Fourier Line Test for Classified Semidirect-Product HSP]]:** Once subgroups have the form $\langle x^m,x^{hm/p}y\rangle$, a two-register QFT recovers the slope $h$ from the measured line condition $ch+d=0\pmod p$.

---

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — HSP precursor.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — abelian HSP / period finding.
- Ettinger--Høyer (1999) — early noncommutative HSP work; in the Zoo as *On Quantum Algorithms for Noncommutative Hidden Subgroups*.
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — lattice connection through dihedral coset problems.
- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes|Gavinsky (2004)]] — HSP for poly-near-Hamiltonian groups.
- [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes|Inui--Le Gall (2007)]] — the $\mathbb{Z}_{p^r}\rtimes\mathbb{Z}_p$ algorithm reused here.
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha (2001)]] — black-box normal-HSP and abelian membership tools behind the semidirect-product algorithms.
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]] — broader PGM/matrix-sum approach for semidirect products.

---

## Cross-links

### Paper notes

- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] — query-efficient HSP for all finite groups; this paper is about efficient post-query processing for a narrow family.
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — broader semidirect-product method.
- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]] — follow-on $\mathbb{Z}_{p^r}\rtimes\mathbb{Z}_{p^2}$ algorithm using the same classification-and-splitting style.
- [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]] — earlier restricted nonabelian HSP by custom Fourier sampling.
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — hard semidirect-product benchmark not solved by this paper.
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — polynomial-space dihedral HSP.
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] — survey context.

### Trick cards

- [[Coprime-Factor Splitting for Semidirect-Product HSP]]
- [[Fourier Line Test for Classified Semidirect-Product HSP]]
