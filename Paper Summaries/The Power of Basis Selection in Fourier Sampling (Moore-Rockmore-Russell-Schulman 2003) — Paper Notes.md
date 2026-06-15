> **Source:** Cristopher Moore, Daniel Rockmore, Alexander Russell, and Leonard J. Schulman, *The Hidden Subgroup Problem in Affine Groups: Basis Selection in Fourier Sampling*, arXiv:quant-ph/0211124 (2003); SODA 2004 version titled *The power of basis selection in Fourier sampling: the hidden subgroup problem in affine groups*
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0211124)
> **Tags:** #hidden-subgroup-problem #nonabelian #fourier-sampling #affine-groups #representation-theory

---

## The computational problem

This paper studies the hidden subgroup problem in the affine and $q$-hedral groups

$$
A_p\cong \mathbb{Z}_{p}^{*}\ltimes\mathbb{Z}_p,
\qquad
\mathbb{Z}_q\ltimes\mathbb{Z}_p\quad(q\mid p-1),
$$

where group elements act as affine maps $x\mapsto ax+b$ over $\mathbb{Z}_p$.

The HSP input is a function $F:G\to S$ that is constant and distinct on left cosets of a hidden subgroup. The central question is not only whether Fourier sampling is used, but which measurement basis is chosen after the nonabelian Fourier transform.

---

## What the paper does

Moore--Rockmore--Russell--Schulman show that strong Fourier sampling can be strictly more informative than weak Fourier sampling and more informative than treating a nonabelian group as if it were abelian.

Main results:

- For prime $p$ and prime $q=(p-1)/\operatorname{polylog}(p)$, the HSP in $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ is solvable in polynomial time by the strong standard method.
- For $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ with any $q\mid p-1$, hidden conjugates are measurement-reconstructible: polynomially many quantum samples contain enough information, though the best classical reconstruction may be exponential.
- Subgroups of the affine group $A_p$ are measurement-reconstructible.
- Abelian Fourier sampling over the direct product $\mathbb{Z}_{p}^{*}\times\mathbb{Z}_p$ fails for the large non-normal affine subgroups: the sample distribution is independent of the translation parameter $b$.
- If the HSP is efficient on $H$ and $K$ has size polynomial in $\log|H|$, then the HSP is efficient on any extension of $K$ by $H$.

---

## Why weak sampling is not enough

The weak standard method measures only the irrep name. It can reconstruct normal subgroups, but for a non-normal subgroup it mainly sees the normal core. In affine groups, the subgroups of interest are conjugates

$$
H_b=(1,b)H(1,-b),
$$

which have the same irrep-name statistics. To recover $b$, the algorithm must measure row/column information in a carefully chosen basis.

The paper frames HSP tractability in three levels:

- **Fully reconstructible:** polynomial quantum circuit plus polynomial classical post-processing finds generators.
- **Measurement reconstructible:** polynomial quantum circuit and specified measurements produce enough data, but classical post-processing may be exponential.
- **Query reconstructible:** the coset states contain enough information under some POVM, with no efficient measurement guaranteed.

This language separates information content, measurement design, and classical decoding.

---

## Affine group setup

The affine group is

$$
A_p=\{(a,b):a\in\mathbb{Z}_p^*,\; b\in\mathbb{Z}_p\},
$$

with multiplication

$$
(a_1,b_1)(a_2,b_2)=(a_1a_2,b_1+a_1b_2).
$$

Let $N\cong\mathbb{Z}_p$ be the normal translation subgroup. The largest non-normal subgroup is

$$
H=\{(a,0):a\in\mathbb{Z}_p^*\}.
$$

Its conjugate $H_b$ is the stabilizer of $b$:

$$
H_b=\{(a,(1-a)b):a\in\mathbb{Z}_p^*\}.
$$

More generally, if $a\in\mathbb{Z}_p^*$ has order $q$, then

$$
H_a=\langle(a,0)\rangle,
\qquad
H_a^b=(1,b)H_a(1,-b).
$$

The nonabelian Fourier transform of $A_p$ has:

- $p-1$ one-dimensional representations inherited from $\mathbb{Z}_p^*$.
- One $(p-1)$-dimensional representation $\rho$ with

$$
\rho((a,b))_{j,k}=
\begin{cases}
\omega_p^{bj}, & k=aj\pmod p,\\
0, & \text{otherwise.}
\end{cases}
$$

For $\mathbb{Z}_q\ltimes\mathbb{Z}_p$, this large representation breaks into $(p-1)/q$ irreps of dimension $q$.

---

## Algorithm for the largest affine stabilizer

For hidden conjugates $H_b$ of the largest non-normal subgroup, the projection in the $(p-1)$-dimensional irrep has rank one. Its columns are phase multiples of

$$
(u_b)_j=\frac{1}{p-1}\omega_p^{bj},\qquad 1\leq j<p.
$$

The measurement is:

1. Perform the nonabelian Fourier transform over $A_p$.
2. Condition on observing the $(p-1)$-dimensional representation $\rho$; this happens with probability $1-1/p$.
3. Measure the column, removing the irrelevant left coset effect.
4. Fourier transform the row index over $\mathbb{Z}_{p-1}$.
5. Measure a frequency $\ell$ and choose the $b$ that makes

$$
\left|\frac{b}{p}-\frac{\ell}{p-1}\right|
$$

smallest.

For each $b$, some $\ell$ is within $1/(2(p-1))$, and that outcome has probability at least $(2/\pi)^2$. Repetition boosts success to high probability.

---

## Large-order $q$-hedral subgroups

For $H_a^b$ where $a$ has order $q$, the projection has nonzero entries only when row and column indices lie in the same coset of $\langle a\rangle\subset\mathbb{Z}_p^*$. Its rank is $(p-1)/q$, but the high-dimensional Fourier data still contains $b$.

After measuring a column and Fourier transforming the row, the probability of a frequency $\ell$ contains sums of the form

$$
\sum_{t=0}^{q-1}\exp\left(i\theta(2a^t k-(p-1))\right),
$$

with $\theta=\pi(bk/p-\ell/(p-1))$ after normalisation. A Gauss-sum argument shows that if

$$
q=p/\operatorname{polylog}(p),
$$

then enough powers $a^t k$ lie in a central interval of $\mathbb{Z}_p$ to give the right frequency with probability at least $1/\operatorname{polylog}(p)$. Thus a polynomial number of trials reconstructs $b$.

For prime $q=(p-1)/\operatorname{polylog}(p)$, the non-normal subgroups are exactly these conjugates, so this gives a full polynomial-time HSP algorithm for the group.

---

## Measurement reconstruction for all $q$-hedral groups

When $q$ is arbitrary, the paper gives an explicit polynomial-size quantum measurement whose outcome distributions for two different $b$ values have constant total variation distance.

The measurement:

1. Measure an irrep $\rho_k$ of dimension $q$.
2. Measure a column.
3. Use a POVM that keeps only a two-dimensional slice indexed by adjacent row labels $u,u+1$.
4. Apply a Hadamard transform on that two-dimensional space.

The resulting probabilities are

$$
\cos^2\left(\frac{\pi m b}{p}\right),\qquad
\sin^2\left(\frac{\pi m b}{p}\right),
$$

where $m$ is uniform over $\mathbb{Z}_p^*$. For any $b\neq b'$, the total variation distance is bounded below by a constant. Therefore $O(\log p)$ samples identify $b$ information-theoretically.

The catch: finding the most likely $b$ from the samples is not known to be polynomial-time in general, matching the dihedral-HSP situation.

---

## Determining subgroup order

To reduce arbitrary subgroups of $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ to hidden conjugates, the paper determines the order of the hidden non-normal cyclic subgroup.

Let

$$
q=\prod_i p_i^{\alpha_i}.
$$

For each prime-power divisor, define homomorphisms

$$
\Upsilon_i^\alpha:G\to \mathbb{Z}_{q/p_i^\alpha}.
$$

The paired oracle $(f,\Upsilon_i^\alpha)$ hides $H\cap A_i^\alpha$, and testing whether this intersection has order $p_i^\alpha$ reveals whether $p_i^\alpha$ divides $|H|$. Once $|H|$ is known, hidden-conjugate reconstruction applies.

---

## Failure of abelian Fourier sampling

If one ignores the semidirect product and Fourier samples over the direct product $\mathbb{Z}_p^*\times\mathbb{Z}_p$, then for the conjugates $H_b$ of the largest non-normal subgroup the measured character probabilities are Gauss sums. For all $b\neq0$, these probabilities are independent of $b$.

So the affine stabilizers cannot be distinguished by the abelian transform. The distinguishing information lies in the high-dimensional nonabelian representation and in the chosen row basis.

---

## Extension closure result

If the HSP is efficient for $H$ and $K$ has size polynomial in $\log|H|$, then any group extension

$$
K\triangleleft G,
\qquad G/K\cong H
$$

also has an efficient HSP algorithm.

Given a hidden subgroup $L\leq G$:

1. Find $L\cap K$ by exhaustive querying inside the small group $K$.
2. Define an oracle on $H$ by collecting all $F$-values on each coset of $K$; this hides $LK/K$.
3. Solve the HSP on $H$.
4. For each generator in the quotient, search its $K$-coset to find a representative in $L$.

This generalizes earlier algorithms for groups with small commutator subgroup and for Hamiltonian groups.

---

## Key results

- Strong Fourier sampling can solve affine/$q$-hedral HSP instances that weak sampling and abelian sampling cannot.
- For $q=(p-1)/\operatorname{polylog}(p)$, hidden conjugates in $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ are fully reconstructible.
- For all $q\mid p-1$, hidden conjugates are measurement-reconstructible.
- Subgroups of $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ and of $A_p$ are measurement-reconstructible.
- Basis choice after the nonabelian Fourier transform is algorithmically significant.

---

## Limits / caveats

- The fully efficient result needs large $q$: $q=p/\operatorname{polylog}(p)$.
- For arbitrary $q$, the measurement data is polynomial in size, but the known classical decoding may be exponential.
- The result does not solve graph isomorphism or the symmetric-group HSP; it mainly shows that chosen bases can matter.
- The affine group has special representation structure: rank-one projections in high-dimensional irreps make the approach work.

---

## Reusable ideas

1. **[[Basis-Selected Strong Fourier Sampling for Affine HSP]]:** Choose a representation basis matched to the hidden conjugate parameter; measuring rows after a second Fourier transform recovers translation data.
2. **[[Two-Dimensional POVM for q-Hedral Hidden Conjugates]]:** Reduce a $q$-dimensional irrep sample to adjacent row labels; a Hadamard turns the hidden shift into $\cos^2/\sin^2$ statistics with constant separation.
3. **[[Small-Kernel Extension Closure for HSP]]:** If a normal kernel is small and the quotient HSP is efficient, reconstruct the hidden subgroup by solving the quotient problem plus coset searches in the kernel.

---

## Cross-links

### Paper notes

- [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]] — earlier efficient nonabelian HSP for wreath products.
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — later semidirect-product HSP framework; cites Moore--Rockmore--Russell--Schulman as a basis-selection result.
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] — query reconstruction versus efficient measurement and decoding.
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — dihedral case, where hidden conjugate decoding is hard.
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — polynomial-space dihedral sieve.
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] — survey context for nonabelian HSP.

### Trick cards

- [[Basis-Selected Strong Fourier Sampling for Affine HSP]]
- [[Two-Dimensional POVM for q-Hedral Hidden Conjugates]]
- [[Small-Kernel Extension Closure for HSP]]
