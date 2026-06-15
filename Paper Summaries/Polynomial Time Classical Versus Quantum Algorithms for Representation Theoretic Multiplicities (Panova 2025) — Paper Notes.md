# Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes

> **Source:** Greta Panova, *Polynomial time classical versus quantum algorithms for representation theoretic multiplicities*, arXiv:2502.20253 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2502.20253) · [PDF](https://arxiv.org/pdf/2502.20253)
> **Tags:** #representation-theory #counting-complexity #kronecker-coefficients #plethysm #classical-algorithms

---

## The computational problem

The paper studies exact computation of standard representation-theoretic multiplicities, with input partitions encoded in unary unless stated otherwise.

For partitions $\lambda,\mu,\nu\vdash n$, the **Kronecker coefficient** $g(\lambda,\mu,\nu)$ is defined by
$$
S_\mu\otimes S_\nu = \bigoplus_{\lambda\vdash n} S_\lambda^{\oplus g(\lambda,\mu,\nu)},
$$
where $S_\lambda$ is the Specht module of $S_n$.

For partitions $\lambda\vdash dm$, the paper focuses on the basic **plethysm coefficient** $a^\lambda_{d,m}$, the coefficient of $s_\lambda$ in
$$
s_{(d)}[s_{(m)}]=h_d[h_m]=\sum_{\lambda\vdash dm} a^\lambda_{d,m}s_\lambda .
$$
It also discusses Kostka numbers $K_{\lambda,\mu}$ and Littlewood--Richardson coefficients $c^\lambda_{\mu\nu}$ as comparison cases, since these already have positive tableau formulas.

The computational question is not merely whether these quantities are in $\mathrm{GapP}_{\geq 0}$ or $\#\mathrm{BQP}$. The sharper question is: in the parameter regimes where the quantum algorithms of [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes|Larocca--Havlicek (2025)]] and [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes|Bravyi--Chowdhury--Gosset--Havlicek--Zhu (2024)]] run in polynomial time, are there already polynomial-time classical algorithms?

## What the paper does

Panova shows that several proposed quantum-advantage regimes for representation-theoretic multiplicities collapse classically. If one Kronecker input has polynomial Specht dimension, $f_\nu\le n^k$, then $g(\lambda,\mu,\nu)$ is computable in polynomial time. For the basic plethysm coefficient $a^\lambda_{d,m}$, polynomial-time classical algorithms exist when either $d$ and $\ell(\lambda)$ are fixed, or $f_\lambda\le n^k$.

My assessment: this is a useful correction paper. It does not kill every possible quantum speedup for multiplicities, but it removes the most obvious low-dimensional regimes. The remaining interesting cases need dimension ratios that stay polynomial while the individual representation dimensions are not polynomial; that is a much narrower target.

## The algorithm / construction

### 1. Classify polynomial-dimensional Specht modules

Define
$$
\operatorname{aft}(\lambda)=|\lambda|-\max\{\lambda_1,\ell(\lambda)\}.
$$
Since $f_\lambda=f_{\lambda'}$, the paper usually assumes $\lambda_1\ge \ell(\lambda)$, so $\operatorname{aft}(\lambda)=n-\lambda_1$.

The key structural fact is that polynomial Specht dimension forces bounded aft. If $f_\lambda\le n^k$, then $\operatorname{aft}(\lambda)\le 4k^2$ for large enough $n$. Intuitively: a partition has only polynomially many standard Young tableaux only when it is a long first row or long first column plus a bounded-size tail.

This is the gatekeeper for the rest of the paper. Once $\operatorname{aft}(\nu)$ is bounded, identities that usually sum over exponentially many partitions collapse to bounded-tail sums.

### 2. Reduce Kronecker computation to bounded-tail multi-LR sums

Start from the Cauchy-style identity
$$
s_\nu[xy]=\sum_{\lambda,\mu} g(\lambda,\mu,\nu)s_\lambda(x)s_\mu(y).
$$
Use Jacobi--Trudi on $s_\nu$:
$$
s_\nu[xy]=\sum_{\sigma\in S_\ell}\operatorname{sgn}(\sigma)
\prod_i h_{\nu_i+\sigma_i-i}[xy].
$$
Because $h_m=s_{(m)}$ and $g((m),\alpha,\beta)=1$ iff $\alpha=\beta$, expanding each $h_m[xy]$ gives
$$
g(\lambda,\mu,\nu)=
\sum_{\sigma\in S_\ell}\operatorname{sgn}(\sigma)
\sum_{\alpha_i\vdash \nu_i+\sigma_i-i}
 c^\lambda_{\alpha_1,\ldots,\alpha_\ell}
 c^\mu_{\alpha_1,\ldots,\alpha_\ell}.
$$
When $\nu_1\ge n-k$, all parts except $\alpha_1$ have total size at most $k$. The algorithm enumerates those small partitions and enumerates $\alpha_1$ by its bounded difference from $\lambda$ or $\mu$. Each multi-Littlewood--Richardson coefficient is computed by iterating over bounded-size intermediate partitions and using a polynomial-time skew Kostka / LR routine for bounded aft.

### 3. Extract basic plethysm coefficients by fixed-dimensional lattice counting

For $a^\lambda_{d,m}$, write $k=\ell(\lambda)$ and work in $k$ variables. Expand
$$
h_d[h_m(x_1,\ldots,x_k)]
$$
by ordering the compositions $b=(b_1,\ldots,b_k)$ of $m$ lexicographically. The terms are grouped by:

- $r$, the number of distinct monomials chosen from $h_m$;
- a strong composition $c_1+\cdots+c_r=d$;
- a vector $\bar j\in[k]^{r-1}$ recording the first coordinate where consecutive compositions differ.

Multiplying by the Vandermonde determinant and comparing coefficients via Weyl's formula gives
$$
a^\lambda_{d,m}=\sum_{\sigma\in S_k}\operatorname{sgn}(\sigma)
\sum_{r=1}^d
\sum_{c_1+\cdots+c_r=d}
\sum_{\bar j\in[k]^{r-1}}
|Q(\bar j,c,\lambda+\delta(k)-\sigma(\delta(k)))|,
$$
where $Q(\bar j,c,\alpha)$ is a rational polytope describing integer matrices $(b^i_j)$ satisfying lexicographic order constraints, composition constraints, and
$$
c_1b^1+\cdots+c_rb^r=\alpha .
$$
If $d$ and $k$ are fixed, Barvinok's algorithm counts these integer points in polynomial time.

### 4. Stabilise the bounded-aft plethysm case

If $\operatorname{aft}(\lambda)=K$ and $\lambda_1\ge\ell(\lambda)$, then only $K+1$ variables matter. For $d>4K^3$, the coefficient formula above forces one selected composition to be $(m,0,\ldots,0)$ with large multiplicity $c_r\ge d-4K^3$. The remaining part has bounded support, so the algorithm replaces the large plethysm count by a bounded-dimensional lattice-counting problem. This recovers a polynomial-time algorithm whenever $f_\lambda\le n^k$, because then $K\le 4k^2$.

## Key results

**Proposition 3.1.** If $\operatorname{aft}(\lambda)=k$, then
$$
\binom{n-k}{k}\le f_\lambda\le \frac{n^k}{\sqrt{k!}} .
$$

**Theorem 1.1.** Let $\lambda,\mu,\nu\vdash n$ and suppose $f_\nu\le n^k$. Then $g(\lambda,\mu,\nu)$ can be computed in time
$$
O\!\left(D(k)n^{4k^2+1}\log n\right),
\qquad
D(k)=(4k)^{8(8k^4+k^2)} .
$$
For fixed $k$, this is polynomial in $n$.

**Proposition 5.1.** If $k=n-\nu_1=\operatorname{aft}(\nu)$, then $g(\lambda,\mu,\nu)$ can be computed in time
$$
O\!\left(
(\ell(\lambda)\log\lambda_1+\ell(\mu)\log\mu_1)
\min\{\ell(\lambda),\ell(\mu)\}^k k^{k^2+2k}
\right).
$$
This is the actual bounded-tail Kronecker algorithm; Theorem 1.1 follows by setting $k\leftarrow 4k^2$ after the dimension-growth lemma.

**Theorem 1.2.** Let $n=dm$ and $\lambda\vdash n$ with $\lambda_1\ge \ell(\lambda)$. Then $a^\lambda_{d,m}$ can be computed in time:
$$
O\!\left(n^{d\ell}\log d\right),\qquad \ell=\ell(\lambda),
$$
when $d$ and $\ell$ are treated as fixed, and in time
$$
O\!\left(n^{4K^3(K+1)}\log d\right),
\qquad K=4k^2,
$$
when $f_\lambda\le n^k$ and $d,m$ are arbitrary.

**Proposition 6.1.** For $\ell(\lambda)=k$, $a^\lambda_{d,m}$ can be computed in time
$$
O\!\left(k!(2k)^{d-1}(dm)^{dk}\log(dk)\right).
$$

## Comparison with prior work

| Work | Claim / result | Panova's relation |
|---|---|---|
| [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes|Bravyi--Chowdhury--Gosset--Havlicek--Zhu (2024)]] | Kronecker coefficients are scaled ranks of efficient representation projectors; positivity is in QMA | Supplies the quantum-complexity context, but not a fast exact algorithm in general |
| [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes|Larocca--Havlicek (2025)]] | Quantum algorithms for multiplicities with runtimes governed by ratios such as $f_\mu f_\nu/f_\lambda$ and $f_\lambda/((f_\nu)^{|\mu|}f_\mu)$ | Panova refutes their Conjecture 2 for Kronecker coefficients with a polynomial-dimensional input, and refutes part of Conjecture 1 for $a^\lambda_{d,m}$ |
| Christandl--Doran--Walter (2012), Pak--Panova (2017) | Classical polynomial algorithms for multiplicities in bounded-row regimes | Panova covers cases where two Kronecker inputs may have unbounded lengths, as long as the third has polynomial dimension |
| Ikenmeyer (2016) | $c^\lambda_{\mu\nu}$ computable in $O((c^\lambda_{\mu\nu})^2\operatorname{poly}(n))$ | Leaves only a narrow window for LR-based quantum speedups when coefficients are not already small |
| Fischer--Ikenmeyer (2020) | Plethysm coefficients are hard in general, already for small inner size | Panova's algorithms are parameter-regime results, not a general polynomial algorithm |

## Limits / caveats

- The paper is mainly a negative result for proposed quantum speedups. It does not give a full classical analogue of all quantum multiplicity algorithms.
- The Kronecker theorem covers $f_\nu\le n^k$. It does not settle regimes where $f_\lambda,f_\mu,f_\nu$ are all superpolynomial but ratios such as $f_\mu f_\nu/f_\lambda$ remain polynomial.
- The constants are enormous. $D(k)=(4k)^{8(8k^4+k^2)}$ is fine for complexity classification, not computation.
- For plethysm, the clean theorem is for $a^\lambda_{d,m}$, i.e. $s_{(d)}[s_{(m)}]$. General $a^\lambda_{\mu,\nu}$ is discussed but not fully solved.
- The algorithms use classical algebraic-combinatorial machinery: Jacobi--Trudi, LR expansions, and Barvinok counting. They are not intended as practical implementations.

## Reusable ideas

1. [[Aft-Bounded Partition Regime for Polynomial Specht Dimension]] — classify polynomial-dimensional symmetric-group irreps by bounded tail size.
2. [[Bounded-Tail Kronecker Expansion via Jacobi-Trudi]] — compute Kronecker coefficients by reducing a long-row input to a bounded sum of multi-LR coefficients.
3. [[Plethysm Coefficient Extraction by Ordered-Composition Polytopes]] — turn $a^\lambda_{d,m}$ into fixed-dimensional integer-point counts after lexicographically stratifying monomials.

## References within this paper

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes|Bravyi--Chowdhury--Gosset--Havlicek--Zhu (2024)]] — source of the quantum projector-rank viewpoint for Kronecker coefficients.
- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes|Larocca--Havlicek (2025)]], *Quantum algorithms for representation-theoretic multiplicities* — main target of the comparison; several conjectured separations are narrowed or refuted here.
- Christandl--Doran--Walter (2012), *Computing multiplicities of Lie group representations* — fixed-rank multiplicity algorithms.
- Pak--Panova (2017), *On the complexity of computing Kronecker coefficients* — prior classical algorithms and complexity results for Kronecker coefficients.
- Ikenmeyer--Mulmuley--Walter (2017), *On vanishing of Kronecker coefficients* — NP-hardness of Kronecker positivity.
- Fischer--Ikenmeyer (2020), *The computational complexity of plethysm coefficients* — general hardness of plethysm.
- Kahle--Michalek (2016), *Plethysm and lattice point counting* — Barvinok-style lattice-point view for plethysm.
- Brion (1993) and Colmenarejo (2017) — stability of plethysm in bounded-tail regimes.
- Ikenmeyer (2016), *Small Littlewood--Richardson coefficients* — classical runtime in terms of the LR coefficient size.

## Cross-links

### Paper notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]

### Trick cards

- [[Aft-Bounded Partition Regime for Polynomial Specht Dimension]]
- [[Bounded-Tail Kronecker Expansion via Jacobi-Trudi]]
- [[Plethysm Coefficient Extraction by Ordered-Composition Polytopes]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Generalized Phase Estimation for Representation Projectors]]
