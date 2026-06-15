# Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes

> **Source:** Jintai Ding, Vlad Gheorghiu, András Gilyén, Sean Hallgren, and Jianqiang Li, *Limitations of the Macaulay matrix approach for using the HHL algorithm to solve multivariate polynomial systems*, arXiv:2111.00405, Quantum 7, 1069 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2111.00405) · [PDF](https://arxiv.org/pdf/2111.00405)
> **Tags:** #quantum-algorithms #HHL #polynomial-systems #macaulay-matrix #cryptanalysis #condition-number

---

## The computational problem

The paper analyses the HHL/Macaulay approach to solving systems of Boolean polynomial equations proposed in [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]] and extended in [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]].

The base problem is:

**Boolean quadratic polynomial system solving.** Given
$$
F=\{f_1,\ldots,f_m\}\subseteq \mathbb F_2[x_1,
\ldots,x_n],\qquad \deg(f_i)\le 2,
$$
find $s\in\mathbb F_2^n$ such that
$$
f_1(s)=\cdots=f_m(s)=0,
$$
when such an $s$ exists.

The HHL-compatible intermediate problem is over $\mathbb C$:

**Boolean solutions over $\mathbb C$.** Given quadratic polynomials
$$
F\subseteq \mathbb C[x_1,\ldots,x_n],
$$
find $s\in\{0,1\}^n$ such that every $f\in F$ vanishes at $s$. The field equations
$$
F_2=\{x_1^2-x_1,\ldots,x_n^2-x_n\}
$$
force Boolean solutions.

The complexity parameter under attack is the condition number of the Macaulay-derived linear system used as input to [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] / QLSA. The paper also studies the **truncated QLS condition number**
$$
\kappa_b(A):=\frac{\|A\|\,\|A^+b\|}{\|b\|},
$$
which lower-bounds what truncated QLSA variants can do on the particular right-hand side $b$.

## What the paper does

This is the technical caveat that the original Chen--Gao cryptanalytic story needed. It proves that the original Macaulay-HHL construction has exponentially large condition number as a function of the Hamming weight $h$ of a Boolean solution, so generic Grover search over assignments of weight $h$ beats it in the main regimes.

It is not just a negative paper. The authors also introduce a reduced **Boolean Macaulay matrix over $\mathbb C$** that removes redundant non-multilinear columns using the Boolean field equations. That lowers the generic lower bound from roughly $(3n)^{h/2}$ to roughly $2^{h/2}$, leaving a narrow possible window for speedups when $h=\Theta(\log n)$ and the actual condition number is polynomial.

My assessment: this mostly defangs the earlier “HHL attacks algebraic cryptography” reading. The refined Boolean Macaulay construction is still interesting, but the paper gives evidence that any win must come from special structure or preconditioning, not from blindly applying HHL to Macaulay matrices.

## The algorithm / construction

### 1. Put the HHL/Macaulay claim under a condition-number microscope

Start with the Macaulay matrix $\widehat M$ of degree $d$ for $F\cup F_2$. Each row is indexed by a pair $(\widehat m,f)$ and contains the coefficient vector of $\widehat m f$, for monomials $\widehat m$ such that $\widehat m f$ has degree at most $d$.

Writing the constant column separately,
$$
\widehat M=[M\mid -b],
$$
gives the Macaulay linear system
$$
My=b.
$$
Chen--Gao use max degree $d=3n$ and show that, for a unique Boolean solution, the QLSA output state corresponds to the unique monomial solution vector.

For a Boolean solution $a\in\{0,1\}^n$, the monomial solution vector has coordinates
$$
y(a)_e=\prod_i a_i^{e_i}.
$$
If $a$ has Hamming weight $h$, then the number of nonzero monomial coordinates is:

- max-degree Macaulay:
  $$
  \|y(a)\|^2=(d+1)^h-1;
  $$
- total-degree Macaulay:
  $$
  \|y(a)\|^2=\binom{d+h}{h}-1.
  $$

Since
$$
\kappa_b(M)=\frac{\|M\|\,\|M^+b\|}{\|b\|}\ge \|M^+b\|,
$$
lower-bounding the norm of the minimum-norm solution already lower-bounds even truncated QLSA performance.

### 2. Compare directly against Grover search by Hamming weight

If the unique Boolean solution has Hamming weight $h$, then Grover search over all assignments of weight $h$ costs
$$
O\!\left(\sqrt{\binom nh}\right)
$$
polynomial-time checks. If $h$ is unknown, the paper notes a Grover variant over growing Hamming-weight layers with cost
$$
O\!\left(h^{1/4}\sqrt{\binom nh}\right).
$$
For Chen--Gao's max-degree $d=3n$ construction, the condition-number lower bound is
$$
\kappa_b(M)\ge (3n)^{h/2}
$$
for a unique solution, which is never the right side of a convincing speedup claim.

### 3. Reduce to the Boolean Macaulay matrix over $\mathbb C$

The paper then exploits the Boolean equations $x_i^2-x_i=0$ more aggressively. Define the multilinearisation map
$$
\psi\!\left(\prod_i x_i^{a_i}\right)=\prod_i x_i^{\min(a_i,1)}
$$
and extend linearly over $\mathbb C[x_1,\ldots,x_n]$.

The **Boolean Macaulay matrix** $\widehat B$ of degree $d$ has:

- rows indexed by pairs $(m,f)$ where $m$ is multilinear and $f\in F_1$;
- columns indexed by multilinear monomials of degree at most $d$;
- row $(m,f)$ equal to the coefficient vector of
  $$
  \psi(mf).
  $$

The ordinary Macaulay matrix for $F_1\cup F_2$ can be row-reduced into the block form
$$
\widehat M'
=\begin{pmatrix}
0 & \widehat B\\
I_2 & B_2
\end{pmatrix},
$$
where $I_2$ handles non-multilinear monomials eliminated by the field equations. Thus solving the Boolean Macaulay linear system
$$
M_B y=b_B,
\qquad \widehat B=[M_B\mid -b_B],
$$
gives a solution of the original Macaulay linear system.

The useful side effect is size and conditioning: the matrix has only multilinear columns, so a Hamming-weight-$h$ solution has $2^h-1$ nonzero monomial coordinates rather than $(d+1)^h-1$.

### 4. Extract the solution by subset-sampling

For a unique Boolean solution $a$, let
$$
S=\{x_i:a_i=1\}.
$$
The QLSA output for the Boolean Macaulay system is the uniform state over nonempty subsets of $S$ of size at most $d$:
$$
|y\rangle=rac{1}{\sqrt{|S_d|}}
\sum_{R\in S_d}|R\rangle,
$$
where
$$
S_d=\{R\subseteq S:1\le |R|\le d\}.
$$
Measuring gives a random subset $R\subseteq S$. Repeating and taking the union recovers all elements of $S$. The paper frames this as a variant of quantum coupon collection.

For general instances with multiple solutions, the authors use Valiant--Vazirani affine hashing: add random affine equations to isolate a unique solution with constant probability, trying $O(n\log(1/\epsilon))$ systems when the number of solutions is unknown.

## Key results

**Truncated QLS condition number.** For a QLSP $Ax=b$, define
$$
\kappa_b(A):=\frac{\|A\|\,\|A^+b\|}{\|b\|}.
$$
This is at most the usual condition number $\kappa(A)=\|A\|\|A^+\|$, but it still lower-bounds the useful part of the inversion needed for this right-hand side. If a truncated QLSA inverts only singular values above $c/\kappa_b(A)$, the solution's overlap with that well-conditioned subspace is small for large $c$.

**Theorem 4.5 / Macaulay lower bound.** Suppose $a_1,\ldots,a_t\in\{0,1\}^n$ are the $t$ Boolean solutions, all of Hamming weight $h$, or else the minimum-norm solution $M^+b$ lies in the convex hull of their monomial solution vectors. Then the Macaulay system has
$$
\kappa_b(M)
\ge
\sqrt{\frac{(d+1)^h-1}{t}}
$$
for max degree, and
$$
\kappa_b(M)
\ge
\sqrt{\frac{\binom{d+h}{h}-1}{t}}
$$
for total degree. In the Chen--Gao setup with max degree $d=3n$,
$$
\kappa_b(M)\ge \sqrt{\frac{(3n)^h}{t}}.
$$
For a unique solution this is $\Omega((3n)^{h/2})$.

**Theorem 4.7 / general solution-set lower-bound formula.** If every Boolean solution has Hamming weight at least $h$ and $d\ge n$, the paper lower-bounds $\kappa_b(M)$ by the shortest vector in a symmetrised affine hull. The bound is
$$
\frac{1}{\langle 1,(G^{(h)})^{-1}1\rangle}
=
\max\{\gamma:G^{(h)}-\gamma 11^T\succeq 0\},
$$
where
$$
G_{ij}=\sum_{s=1}^n c_s^d
\binom{i}{s}\binom{j}{s}\bigg/\binom ns,
$$
with $c_s^d=d^s$ for max degree and $c_s^d=\binom ds$ for total degree. The authors report symbolic checks that for max degree $d=3n$, the expression is at least $h^{h/2}$ for all $h\in[n]$ up to $n=300$.

**Lemma 5.4 / sparsity of the Boolean Macaulay matrix.** The Boolean Macaulay matrix $\widehat B$ of total degree $d$ is
$$
O(m\cdot \#F_1)
$$
sparse, and its nonzero locations and values are efficiently computable. This is what keeps the refined construction compatible with QLSA input oracles.

**Lemma 5.5 / equivalence of ordinary and Boolean Macaulay systems.** A solution of the Boolean Macaulay linear system corresponds to a solution of the ordinary Macaulay linear system after the row-reduction block decomposition
$$
\widehat M'
=\begin{pmatrix}
0 & \widehat B\\
I_2 & B_2
\end{pmatrix}.
$$
The note in the paper also implies that the complete solving degree in the original Chen--Gao approach is at most $n+2$, improving the earlier $3n$ upper bound.

**Corollary 5.6 / Boolean Macaulay lower bound.** For the Boolean Macaulay matrix $\widehat B=[M\mid -b]$, under the same equal-weight or convex-hull assumption,
$$
\kappa_b(M)\ge \frac12\sqrt{\frac{2^h-1}{t}}.
$$
For $h=\Theta(\log n)$ this lower bound is only polynomial, so the paper cannot rule out a speedup in that special regime.

**Theorem 6.2 / subset coupon collection.** Let $S\subseteq U$ and let $S_d$ be the nonempty subsets of $S$ of size at most $d$. Given copies of
$$
|y\rangle=\frac{1}{\sqrt{|S_d|}}
\sum_{R\in S_d}|R\rangle,
$$
measuring
$$
r=O\!\left(\frac{|S|}{d}\log\frac{|S|}{\epsilon}\right)
$$
copies suffices to recover $S$ with probability at least $1-\epsilon$.

**Lemma 6.3 / refined quantum algorithm.** For a unique-solution Boolean system over $\mathbb C$, the Boolean Macaulay QLSA algorithm solves Problem 3.2 with high probability in time
$$
\widetilde O(\operatorname{poly}(n)\kappa(M)),
$$
using the improved QLSA scaling from Childs--Kothari--Somma rather than the original $\kappa^2$ HHL dependence. For non-unique systems, Valiant--Vazirani hashing adds an $O(n)$ outer repetition factor; with $O(\log n)$ subset-sampling rounds, the paper quotes $O(n\log n)$ total iterations.

## Comparison with prior work

| Work / approach | What it claims or gives | What this paper changes |
|---|---|---|
| [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes|Chen--Gao (2018)]] | HHL on a Macaulay system; polynomial in $n,T_F$ if $\kappa$ is small; max degree $3n$ | Shows the induced condition number is generically enormous: $\Omega((3n)^{h/2})$ for a unique weight-$h$ solution. |
| [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes|Chen--Gao--Yuan (2018)]] | Reduces finite-field solving and cryptanalytic optimization to the Chen--Gao Boolean solver | The condition-number caveat carries over; reductions may also increase variables enough to erase the narrow $h=\Theta(\log n)$ window. |
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] over assignments | $O(\sqrt{\binom nh})$ checks when the solution weight is $h$ | For the original Macaulay construction, Grover is at least as good and often strictly better. |
| Classical Gröbner / XL / Macaulay methods | Eliminate variables by Gaussian elimination on large Macaulay matrices | The paper keeps the HHL idea but shows that matrix conditioning, not just dimension, blocks the advertised win. |
| Modern QLSA / [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] methods | Improve precision and condition-number dependence, often to near-linear in $\kappa$ | The lower bounds use $\kappa_b(M)$, so they also hit truncated QLSA approaches for this right-hand side. |

## Limits / caveats

1. **This is mostly a lower-bound diagnosis, not a full no-go theorem.** The Boolean Macaulay construction still leaves open a possible polynomial-condition-number family when $h=\Theta(\log n)$.

2. **The lower bound is about known QLSA-style access.** Preconditioning is not ruled out. The discussion explicitly lists preconditioned QLSA as an open direction.

3. **Multiple solutions are harder.** The clean theorem covers equal Hamming weights or a convex-hull condition. The paper gives a general SDP/Gram lower-bound formula and computational evidence, but not a simple closed-form theorem for every mixed-weight solution set.

4. **The refined algorithm still has a condition number in the runtime.** The Boolean Macaulay matrix is smaller and better motivated, but the paper does not exhibit a natural cryptanalytic family where $\kappa(M)=\operatorname{poly}(n)$.

5. **The cryptanalytic reading is conservative.** For typical algebraic attacks with one or few solutions, the evidence points against a useful HHL/Macaulay attack. The possible $h=\Theta(\log n)$ window is not typical for full-key recovery encodings.

## Reusable ideas

1. [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test]] — lower-bound QLSA performance using $\|A^+b\|$ rather than the full condition number.
2. [[Hamming-Weight Norm Lower Bounds for Macaulay Solution States]] — count nonzero monomial coordinates of a Boolean assignment to certify large Macaulay solution-state norm.
3. [[Boolean Macaulay Reduction over C]] — row-reduce away non-multilinear monomial columns using $x_i^2-x_i=0$ while preserving Boolean solutions.
4. [[Monomial-Subset Coupon Collection for Boolean Solution Readout]] — recover the support of a Boolean solution by repeatedly measuring subset labels from a monomial superposition.

## References within this paper

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes|Chen--Gao (2018)]] — the main target; proposes the original Macaulay-HHL Boolean solver.
- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes|Chen--Gao--Yuan (2018)]] — finite-field and optimization extension of the same framework.
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow--Hassidim--Lloyd (2009)]] — original QLSA and source of the condition-number bottleneck.
- Childs--Kothari--Somma (2017) — QLSA with improved precision dependence; used for the refined algorithm runtime.
- Ambainis (2012), Chakraborty--Gilyén--Jeffery (2019), Gilyén--Su--Low--Wiebe (2019), Subaşı--Somma--Orsucci (2019), Lin--Tong (2020) — improved QLSA and block-encoding context.
- Aaronson (2015) — caveat that HHL applications must account for state preparation, readout, sparsity, and conditioning.
- Valiant--Vazirani (1986) — affine hashing used to isolate a unique solution.
- Arunachalam--Belovs--Childs--Kothari--Rosmanis--de Wolf (2020) — quantum coupon collector, generalised here to subset samples.
- Bardet--Faugère--Salvy--Spaenlehauer (2013), Courtois--Klimov--Patarin--Shamir (2004), Diem (2004) — XL / Boolean Macaulay / polynomial-solving background.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] and Boyer--Brassard--Høyer--Tapp (1998) — quantum search baselines.

## Cross-links

### Paper notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]

### Trick cards

- [[Macaulay-HHL Pseudo-Solution States]]
- [[Boolean Solution Extraction by Monomial Measurement]]
- [[Complete Solving Degree for Monomial Recovery]]
- [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test]]
- [[Hamming-Weight Norm Lower Bounds for Macaulay Solution States]]
- [[Boolean Macaulay Reduction over C]]
- [[Monomial-Subset Coupon Collection for Boolean Solution Readout]]
