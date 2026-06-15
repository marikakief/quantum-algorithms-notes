# Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes

> **Source:** Harry Buhrman and Robert Špalek, *Quantum Verification of Matrix Products*, arXiv:quant-ph/0409035, SODA 2006
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0409035) · [PDF](https://arxiv.org/pdf/quant-ph/0409035)
> **Tags:** #quantum-walks #matrix-multiplication #verification #query-complexity #linear-algebra

---

## The computational problem

**Matrix product verification.** Given three $n \times n$ matrices $A,B,C$ over an integral domain, decide whether

$$
AB=C.
$$

The paper measures both query complexity and time, assuming entries of $A,B,C$ can be queried in unit time. The output is one-sided bounded-error: always accept if $AB=C$, and reject with probability at least $2/3$ if $AB\ne C$.

The paper also studies a related output problem: compute $C=AB$, especially when the product has few nonzero entries.

---

## What the paper does

Buhrman and Špalek improve quantum matrix product verification from the $O(n^{7/4})$ recursive-Grover method of [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes|Ambainis-Buhrman-Høyer-Karpinski-Kurur]] to worst-case $O(n^{5/3})$ time and $O(n)$ space. The real contribution is not just applying the [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|element-distinctness walk]]: a direct query-optimal walk would still spend $\Omega(n^2)$ time loading submatrices. They fix that with a bilinear random compression that makes the phase check cost $O(n)$ rather than $O(kn)$.

My assessment: this is a neat early example where the query algorithm is not enough. The paper is really about turning a quantum-walk query speedup into an actual time speedup by engineering the data maintained at each Johnson-graph vertex.

---

## The algorithm / construction

Let $W=\{(i,j):(AB-C)_{i,j}\ne 0\}$ be the set of wrong entries. A walk vertex is a pair $(R,S)$ where $R,S\subseteq[n]$ and $|R|=|S|=k$.

### Product Verification

The outer algorithm tries geometrically increasing subset sizes $k$.

1. Choose any $1<\lambda<8/7$, e.g. $\lambda=15/14$.
2. For $i=0,1,\ldots,\log_\lambda(n^{2/3})+9$, repeat 16 times:
   - run `Verify Once` with
     $$
     k=\sqrt[3]{8}\,\lambda^i;
     $$
   - if it detects an error, output “not equal”.
3. If no run detects an error, output “equal”.

The 16 repetitions compensate for the random compression hiding a marked submatrix with constant probability.

### Verify Once

For a chosen $k$:

1. Pick a walk length $\ell$ uniformly from $\{1,\ldots,k\}$.
2. Pick a random row vector $p$ and random column vector $q$, each of length $n$.
3. Initialise the quantum register in the uniform superposition over pairs of $k$-subsets:
   $$
   \sum_{R\in {[n]\choose k}}\sum_{S\in {[n]\choose k}} |R\rangle |S\rangle,
   $$
   ignoring normalisation.
4. For each basis state $(R,S)$, compute and store
   $$
   a_R=p|_R A|_R,\qquad b_S=B|_S q|_S,\qquad c_{R,S}=p|_R C^S_R q|_S.
   $$
   This costs $2kn+k^2$ time.
5. Perform $\ell$ iterations of a [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walk]] on
   $$
   G=J(n,k)\times J(n,k),
   $$
   the categorical product of two Johnson graphs:
   - **Phase flip:** multiply by $-1$ iff
     $$
     a_R\cdot b_S\ne c_{R,S}.
     $$
     This scalar-product check costs $O(n)$ time.
   - **Diffusion/update:** exchange one row in $R$ and one column in $S$, updating $a_R,b_S,c_{R,S}$. The update uses $2n$ queries to $A$, $2n$ queries to $B$, and $4k$ queries to $C$, hence $O(n)$ time for $k\le n$.
6. Estimate overlap with the uniform state using a single control qubit: if the walk has moved far from uniform, output “not equal”.

If $AB=C$, no phase flip ever occurs and the uniform state is fixed by the walk, so the algorithm never falsely rejects.

### Why $k=n^{2/3}$ appears

Szegedy's search theorem says marked vertices are detected in

$$
O\left(\frac{1}{\sqrt{\delta_G\epsilon}}\right)
$$

walk steps, where $\delta_G$ is the spectral gap and $\epsilon$ is the marked fraction. For $G=J(n,k)\times J(n,k)$,

$$
\delta_G=\Theta(1/k).
$$

For one wrong entry, a random $(R,S)$ contains it with probability

$$
\epsilon=\Theta(k^2/n^2).
$$

Thus the walk needs $O(n/\sqrt{k})$ steps. Since `Verify Once` uses $\ell\le k$ and each step costs $O(n)$ time, set

$$
k\approx n^{2/3}
$$

to make $k\ge n/\sqrt{k}$. The resulting time is $O(kn)=O(n^{5/3})$.

For many wrong entries, the algorithm can stop earlier. The paper proves this using a combinatorial lower bound on the marked fraction.

---

## Key results

### Matrix product verification

Let $W=\{(i,j):(AB-C)_{i,j}\ne 0\}$, and let $W'$ be the largest independent subset of $W$, meaning at most one wrong entry in each row and column. Define

$$
q(W)=\max\left(|W'|,\min(|W|,\sqrt n)\right).
$$

**Theorem 6.** `Product Verification` always returns “equal” if $AB=C$. If $AB\ne C$, it returns “not equal” with probability at least $2/3$. Its worst-case running time is

$$
O(n^{5/3}),
$$

its expected running time is

$$
O\left(\frac{n^{5/3}}{q(W)^{1/3}}\right),
$$

and its space complexity is $O(n)$.

In the abstract this is stated more coarsely as

$$
O\left(\frac{n^{5/3}}{\min(w,\sqrt n)^{1/3}}\right),
$$

where $w=|W|$.

### Marked-pair lower bound

For $k\le n^{2/3}/q(W)^{1/3}$, the fraction of marked subset pairs satisfies

$$
\epsilon(W,k)=\Omega\left(\frac{k^2}{n^2}q(W)\right).
$$

This is the combinatorial part that lets the expected time improve when the error pattern is spread out.

### Lower bound for a wrong row

Any bounded-error quantum algorithm distinguishing a correct product from a product with one wrong row has query complexity

$$
\Omega(n^{3/2}).
$$

The reduction is from OR of $n$ parities of length $n+1$ over $\mathrm{GF}(2)$.

### Sparse-output matrix multiplication

For $m\ge n^{2/3}$, the product $A_{n\times m}B_{m\times n}=C_{n\times n}$ can be computed with polynomially small error in expected time

$$
T_M\le O(1)\cdot
\begin{cases}
 m\log n\cdot n^{2/3}w^{2/3}, & 1\le w\le \sqrt n,\\
 m\log n\cdot \sqrt n\,w, & \sqrt n\le w\le n,\\
 m\log n\cdot n\sqrt w, & n\le w\le n^2,
\end{cases}
$$

where $w$ is the number of nonzero entries of the product. For square matrices, this beats the Coppersmith-Winograd-era $O(n^{2.376})$ dense bound when the output has $w=o(n^{0.876})$ nonzero entries, ignoring logarithms and constants.

### Boolean matrix products

The random-vector compression relies on arithmetic over an integral domain, so it does not apply to Boolean semiring multiplication. The paper gives a separate Boolean verification bound by writing the condition as an AND-OR tree:

$$
O(n\sqrt m)
$$

for verifying an $n\times m$ by $m\times n$ Boolean product.

---

## Comparison with prior work

| Method | Problem | Complexity | Main idea |
|---|---:|---:|---|
| Direct multiplication | compute $AB$ | $O(n^3)$ | naive arithmetic |
| Strassen / Coppersmith-Winograd era | compute $AB$ | $O(n^{2.376})$ | algebraic fast matrix multiplication |
| Freivalds (1979) | verify $AB=C$ | $O(n^2)$ randomized | random right projection $A(Bx)=Cx$ |
| [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes|ABHKK (2002)]] | verify $AB=C$ | $O(n^{7/4})$ | block Freivalds + Grover/amplification |
| This paper | verify $AB=C$ | $O(n^{5/3})$ worst case | Szegedy walk on $J(n,k)^2$ plus bilinear compression |
| This paper | compute sparse-output product | output-sensitive; see Theorem 12 | repeated verification + binary search for wrong entries |

The gap left open is large: the paper proves $\Omega(n^{3/2})$ only for the one-wrong-row promise, while the verification upper bound is $O(n^{5/3})$.

---

## Limits / caveats

- The algorithm is for matrices over an integral domain. The random bilinear compression can fail for semiring products such as Boolean matrix multiplication.
- The best lower bound in the paper is $\Omega(n^{3/2})$ for a special promise, not a matching $\Omega(n^{5/3})$ lower bound.
- The expected-time improvement depends on the geometry of the wrong entries through $q(W)$, not just the number $w=|W|$. Dense but highly clustered errors can be less helpful than many independent errors.
- The matrix multiplication result is output-sensitive. It is compelling only when the product is sufficiently sparse, and it assumes efficient entry queries and arithmetic.
- The phase-flip trick reduces time by storing compressed row/column data, but it introduces constant-probability masking; hence the repeated trials and the $\sqrt[3]{8}$ inflation in $k$.

---

## Reusable ideas

1. [[Johnson-Product Quantum Walk for Matrix Product Verification]] — Walk over pairs of row/column subsets so a wrong matrix entry becomes a marked vertex when both its row and column are selected.
2. [[Random Bilinear Compression for Submatrix Product Tests]] — Replace full submatrix-product checks by random left/right projections $p_R(A_RB_S-C^S_R)q_S$ so the phase test is cheap.
3. [[Sparse-Output Product via Verification and Correction]] — Turn a verifier into an output-sensitive multiplication algorithm by repeatedly finding and correcting wrong entries.

---

## References within this paper

- [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes|Ambainis-Buhrman-Høyer-Karpinski-Kurur (2002)]] — previous $O(n^{7/4})$ quantum verification algorithm.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004/2007)]] — Johnson-graph quantum walk template.
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — general quantum-walk search theorem used in the analysis.
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] — Grover search with an unknown number of solutions, used in sparse-output multiplication.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — baseline search primitive.
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy (2005/2006)]] — cited for the adversary barrier around certificate-complexity lower bounds.
- Freivalds (1979) — classical $O(n^2)$ randomized matrix product verification.
- Coppersmith-Winograd (1990) — dense matrix multiplication comparison point.
- Magniez-Santha-Szegedy (2005) — triangle finding as another Johnson-walk application; now documented in [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]].
- Magniez-Nayak (2005) — group commutativity testing via quantum walks.
- Høyer-Mosca-de Wolf (2003) — quantum search on bounded-error inputs, used for Boolean matrix verification.

---

## Cross-links

### Paper notes

- [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]

### Trick cards

- [[Johnson-Product Quantum Walk for Matrix Product Verification]]
- [[Random Bilinear Compression for Submatrix Product Tests]]
- [[Sparse-Output Product via Verification and Correction]]
