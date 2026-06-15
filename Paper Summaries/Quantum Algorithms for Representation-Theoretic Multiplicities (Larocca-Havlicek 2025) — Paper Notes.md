# Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes

> **Source:** Martín Larocca and Vojtech Havlicek, *Quantum Algorithms for Representation-Theoretic Multiplicities*, arXiv:2407.17649v5 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2407.17649) · [PDF](https://arxiv.org/pdf/2407.17649)
> **Tags:** #quantum-algorithms #representation-theory #symmetric-group #fourier-sampling #multiplicity

---

## The computational problem

The paper studies exact computation of representation-theoretic multiplicities. The common template is:

- Input: finite groups $H \subseteq G$, irrep labels $\alpha \in \widehat G$ and $\beta \in \widehat H$, usually with $G=S_n$ or $S_n\times S_n$ and partitions encoded in unary.
- Output:
  $$
  \operatorname{mult}\left(r_H^\beta, r_G^\alpha\downarrow_H^G\right),
  $$
  the multiplicity of the $H$-irrep $r_H^\beta$ in the restriction of the $G$-irrep $r_G^\alpha$.
- Complexity measure: gate complexity as a function of $n$ and the relevant representation-dimension ratio. The paper assumes an $S_n$ QFT with $O(n^4)$ gates, citing Kawano--Sekigawa.

The four concrete families are:

| Coefficient | Multiplicity formulation | Quantum sampling probability | Efficient when |
|---|---|---|---|
| Kostka $K^\mu_\nu$ | $\operatorname{mult}(r^{\mathrm{triv}}_{S_\mu}, r^\nu_{S_n}\downarrow^{S_n}_{S_\mu})$ | $K^\mu_\nu/d_\nu$ | $d_\nu \leq \operatorname{poly}(n)$ |
| Littlewood--Richardson $c^\nu_{\lambda\mu}$ | $\operatorname{mult}(r^\lambda_{S_a}\otimes r^\mu_{S_b}, r^\nu_{S_n}\downarrow^{S_n}_{S_a\times S_b})$ | $c^\nu_{\lambda\mu}d_\lambda d_\mu/d_\nu$ | $d_\nu/(d_\lambda d_\mu) \leq \operatorname{poly}(n)$ |
| Plethysm $a^\nu_{\lambda\mu}$ | $\operatorname{mult}(r^\lambda_{S_c}\wr r^\mu_{S_d}, r^\nu_{S_n}\downarrow^{S_n}_{S_c\wr S_d})$, $cd=n$ | $a^\nu_{\lambda\mu}d_\lambda^d d_\mu/d_\nu$ | $d_\nu/(d_\lambda^d d_\mu) \leq \operatorname{poly}(n)$ and efficient $S_c\wr S_d$ QFT |
| Kronecker $g_{\lambda\mu\nu}$ | $\operatorname{mult}(r^\nu_{S_n}, r^\lambda_{S_n}\otimes r^\mu_{S_n})$ | $g_{\lambda\mu\nu}d_\nu/(d_\lambda d_\mu)$ | $d_\lambda d_\mu/d_\nu \leq \operatorname{poly}(n)$ |

This paper is the algorithmic companion to [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]], and it is the main target later narrowed by [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]].

## What the paper does

Larocca--Havlicek give a single Fourier-sampling routine that estimates branching multiplicities exactly, with sample complexity governed by an irrep-dimension ratio. Instantiating it gives quantum algorithms for Kostka, Littlewood--Richardson, plethysm, and Kronecker coefficients on regimes where that ratio is polynomial.

My assessment: the algorithmic core is clean and useful, but the speedup claims are now much weaker than the first version suggested. The paper itself acknowledges Panova's 2025 classical algorithm for the Kronecker low-dimensional-input regime, leaving at best polynomial gaps and a more plausible open case around plethysm.

## The algorithm / construction

### Restriction algorithm: restricted-isotypic Fourier sampling

The main routine estimates
$$
\operatorname{mult}(r_H^\beta, r_G^\alpha\downarrow_H^G)
$$
for $H\subseteq G$.

1. **Prepare the $G$-isotypic maximally mixed state.** Let $U_G$ be the $G$-QFT on the regular representation $\mathbb C[G]$. Prepare
   $$
   \rho_G^\alpha
   =U_G^\dagger\left(|\alpha\rangle\!\langle\alpha|\otimes {I_{d_\alpha}\over d_\alpha}\otimes {I_{d_\alpha}\over d_\alpha}\right)U_G
   ={\Pi_G^\alpha\over d_\alpha^2}.
   $$
   This is the maximally mixed state on the $\alpha$-isotypic subspace of the regular representation.

2. **Prepare a uniform subgroup control register.** On a control register $\mathbb C[H]$, prepare
   $$
   {1\over \sqrt{|H|}}\sum_{h\in H}|h\rangle
   $$
   by applying the inverse $H$-QFT to $|\mathrm{triv},1,1\rangle$.

3. **Apply controlled left multiplication.** Apply
   $$
   \sum_{h\in H}|h\rangle\!\langle h|\otimes R(h),
   $$
   where $R$ is the left-regular action of $G$ on $\mathbb C[G]$, restricted to subgroup elements $h\in H$.

4. **Weak Fourier sample the control.** Apply the $H$-QFT to the control register and measure only the irrep label $\beta$.

The measurement operator for outcome $\beta$ is
$$
E_\beta={1\over \sqrt{|H|}}\sum_{h\in H}\left[(|\beta\rangle\!\langle\beta|\otimes I_\beta\otimes I_\beta)V_H|h\rangle\right]\otimes R(h),
$$
where $V_H$ is the $H$-QFT. The character-orthogonality calculation gives
$$
\Pr[\beta]
=\operatorname{Tr}(E_\beta\rho_G^\alpha E_\beta^\dagger)
={d_\beta\over d_\alpha}\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G).
$$
Since the multiplicity is an integer, $N=O((d_\alpha/d_\beta)^2)$ samples suffice to estimate $\Pr[\beta]$ finely enough to round to the exact value with constant success probability.

### Instantiations

**Kostka.** Set $G=S_n$, $H=S_\mu=\prod_i S_{\mu_i}$, $\alpha=\nu$, and $\beta=\mathrm{triv}$. Then
$$
\Pr[\mathrm{triv}]={K^\mu_\nu\over d_\nu},
$$
so the cost is $O(n^4d_\nu^2)$ under the $S_n$ QFT assumption. The authors also give a classical polynomial-time enumeration algorithm when $d_\nu\leq \operatorname{poly}(n)$, using the fact that every Kostka number is at most $d_\nu$.

**Littlewood--Richardson.** Set $G=S_n$, $H=S_a\times S_b$, $\alpha=\nu$, and $\beta=(\lambda,\mu)$. Then
$$
\Pr[(\lambda,\mu)]={c^\nu_{\lambda\mu}d_\lambda d_\mu\over d_\nu},
$$
and the gate complexity is
$$
O\!\left(n^4\left({d_\nu\over d_\lambda d_\mu}\right)^2\right).
$$
They hypothesise that this regime should also have a classical polynomial-time algorithm.

**Plethysm.** Set $G=S_n$, $H=S_c\wr S_d$ with $cd=n$, and use the wreath-product irrep $r^\lambda_{S_c}\wr r^\mu_{S_d}$ of dimension $d_\lambda^d d_\mu$. Then
$$
\Pr[(\lambda,\mu)]={a^\nu_{\lambda\mu}d_\lambda^d d_\mu\over d_\nu}.
$$
The runtime is polynomial only when the dimension ratio is polynomial and the $S_c\wr S_d$ QFT is efficient; the paper cites efficient QFTs only for $|S_c|\leq \operatorname{poly}(n)$, i.e. $c=O(\log n/\log\log n)$ up to constants.

**Kronecker.** Set $G=S_n\times S_n$, $H=S_n$ diagonally embedded, $\alpha=(\lambda,\mu)$, and $\beta=\nu$. The algorithm samples
$$
\Pr[\nu]={g_{\lambda\mu\nu}d_\nu\over d_\lambda d_\mu}
$$
and computes $g_{\lambda\mu\nu}$ in
$$
O\!\left(n^4\left({d_\lambda d_\mu\over d_\nu}\right)^2\right)
$$
gates. For the family $\mu=(n-k,\alpha_1,\alpha_2,\ldots)$ with fixed $k$ and $d_\lambda=d_\nu$, the paper gets $O(n^{2k+4})$ gates, but [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]] gives a classical $\widetilde O(n^{4k^2+1})$ algorithm for this low-dimensional-input regime.

### Induction algorithm

The paper also gives an induction-side version using Frobenius reciprocity:
$$
\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G)
=
\operatorname{mult}(r_G^\alpha,r_H^\beta\uparrow_H^G).
$$
Prepare $\rho_H^\beta$ in $\mathbb C[H]$ and a maximally mixed coset register $\mathbb C[G/H]$. Apply the embedding
$$
U_E: |t\rangle|h\rangle\mapsto |th\rangle
$$
from coset representative plus subgroup element into $\mathbb C[G]$, then weak-Fourier-sample $G$. Outcome $\alpha$ appears with probability
$$
q_\beta(\alpha)= {|H|d_\alpha\over |G|d_\beta}\operatorname{mult}(r_G^\alpha,r_H^\beta\uparrow_H^G).
$$
The sample cost is therefore
$$
O\!\left(\left({d_\beta |G|\over d_\alpha |H|}\right)^2\right),
$$
so the restriction version is preferable when
$$
\left({d_\alpha\over d_\beta}\right)^2 < {|G|\over |H|}.
$$
For $\beta=\mathrm{triv}$ this is exactly weak Fourier sampling of the usual hidden-subgroup coset state; the paper points out the connection, but the multiplicity-estimation task is not the HSP.

## Key results

**Theorem 1 (restriction multiplicity algorithm).** Let $H\subseteq G$. Let $H$ denote the gate size of the $H$-QFT, $R$ the gate size for preparing $\rho_G^\alpha$, and $D$ the gate size for controlled left-regular action by subgroup elements. There is a quantum algorithm that, given irrep labels $(\alpha,\beta)$, computes
$$
\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G)
$$
with constant success probability in
$$
O\!\left((H+R+D)\left({d_\alpha\over d_\beta}\right)^2\right)
$$
time.

The measured distribution is exactly
$$
\Pr[\beta]
={d_\beta\over d_\alpha}
\operatorname{mult}(r_H^\beta,r_G^\alpha\downarrow_H^G).
$$

**Lemma 2 (rounding sample bound).** Since the multiplicity is an integer, $O((d_\alpha/d_\beta)^2)$ shots suffice to estimate $\Pr[\beta]$ to within $d_\beta/(2d_\alpha)$ and round to the correct multiplicity with constant probability.

**Theorem 2 (classical Kostka algorithm in the low-dimensional regime).** Given $\mu,\nu\vdash n$ in unary with $d_\nu\leq \operatorname{poly}(n)$, there is a deterministic polynomial-time algorithm computing $K^\mu_\nu$.

The proof enumerates semistandard Young tableaux of shape $\nu$ and content $\mu$. Fayers' monotonicity lemma implies $K^\mu_\nu\leq d_\nu$, so the enumeration tree has polynomial width.

**Theorem 3 (induced multiplicity algorithm).** If there is a $G$-QFT circuit of size $G$ and a preparation circuit of size $R$ for $\rho_H^\beta$, then there is a quantum algorithm computing
$$
\operatorname{mult}(r_G^\alpha,r_H^\beta\uparrow_H^G)
$$
with high probability in
$$
O\!\left((G+R)\left({d_\beta |G|\over d_\alpha |H|}\right)^2\right).
$$

**Kronecker low-dimensional-input family.** For $\mu=(n-k,\alpha_1,\alpha_2,\ldots)$ with fixed $k$ and $\alpha\vdash k$,
$$
 d_\mu \leq {n^k d_\alpha\over k!}.
$$
If $d_\lambda=d_\nu$, the quantum algorithm computes $g_{\mu\lambda\lambda}$ in
$$
O(n^{2k+4})
$$
gates. Panova's later algorithm computes the same low-dimensional-input Kronecker regime classically in $\widetilde O(n^{4k^2+1})$ according to this paper.

## Comparison with prior work

| Work | Role | Relation to Larocca--Havlicek |
|---|---|---|
| [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes|Bravyi--Chowdhury--Gosset--Havlicek--Zhu (2024)]] | Projector-rank view of Kronecker coefficients; QMA/#BQP-style complexity results | Larocca--Havlicek recast the Kronecker sampling algorithm as one instance of a general branching-multiplicity routine |
| [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes|Jordan (2009)]] | Quantum algorithms for estimating $S_n$ irrep matrix elements and characters | A warning case: similar-looking $S_n$ quantum algorithms had classical counterparts via Roichman-type formulas |
| Beals (1997), Kawano--Sekigawa (2016) | Efficient $S_n$ QFTs | Supply the $O(n^4)$ QFT cost assumption used in the runtime estimates |
| Moore--Russell--Sniady (2007) | QFT over wreath products; graph-isomorphism sieve limits | Supplies the $S_c\wr S_d$ QFT ingredient for plethysm under $|S_c|\leq \operatorname{poly}(n)$ |
| Christandl--Doran--Walter (2012), Pak--Panova (2017), Mishna--Trandafir (2022) | Classical algorithms for Kronecker coefficients in bounded-length or polytope-counting regimes | These results limit where the quantum algorithm could plausibly improve classical methods |
| [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]] | Classical algorithms in regimes targeted by this paper | Narrows the quantum-advantage story sharply, especially for Kronecker and some plethysm regimes |

## Limits / caveats

- **The dimension ratio is everything.** The algorithm is exact, but its sample cost is quadratic in $d_\alpha/d_\beta$ or the relevant induced-ratio analogue. On many natural inputs these dimensions are exponential.

- **Kronecker advantage is no longer compelling as a superpolynomial claim.** The paper's Conjecture 2 was disproved by [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]]. The remaining claimed gap is polynomial if Panova's scaling is tight.

- **Kostka gets classically simulated in the same regime.** The paper proves this itself: when $d_\nu$ is polynomial, direct enumeration of semistandard Young tableaux is polynomial.

- **Littlewood--Richardson is conjecturally classical in the same regime.** They state this as a hypothesis, not as a theorem.

- **Plethysm is the best remaining candidate, but the regime is narrow.** The $S_c\wr S_d$ QFT is only known efficient under a strong restriction on $c$, and [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova]] later finds classical algorithms for several proposed low-dimensional plethysm cases.

- **State preparation hides real work.** The theorem parameter $R$ includes preparing $\rho_G^\alpha$ or $\rho_H^\beta$. The paper mostly treats QFT-based preparation as available; outside the symmetric-group setting this is a serious condition.

## Reusable ideas

1. [[Multiplicity Estimation by Restricted-Isotypic Fourier Sampling]] — estimate branching multiplicities by preparing a $G$-isotypic state and weak-Fourier-sampling over a subgroup.
2. [[Induced-Representation Multiplicity Sampling by Coset Embedding]] — estimate induced multiplicities by embedding $\mathbb C[G/H]\otimes\mathbb C[H]$ into $\mathbb C[G]$ and weak-Fourier-sampling $G$.
3. [[Dimension-Ratio Triage for Multiplicity Algorithms]] — use representation-dimension ratios as the first-pass test for whether a Fourier-sampling multiplicity algorithm has any chance of being efficient.
4. [[Kostka Enumeration from a Specht-Dimension Bound]] — turn a small Specht dimension bound into a polynomial-width enumeration of semistandard Young tableaux.

## References within this paper

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes|Bravyi--Chowdhury--Gosset--Havlicek--Zhu (2024)]] — earlier Kronecker-coefficient quantum-complexity paper and source of the projector-rank viewpoint.
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]] — later classical algorithms that disprove the paper's original Kronecker conjecture and constrain plethysm regimes.
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes|Jordan (2009)]] — related quantum access to irreducible representations of $S_n$ and other groups.
- Beals (1997), *Quantum computation of Fourier transforms over symmetric groups* — original efficient quantum $S_n$ QFT.
- Kawano--Sekigawa (2016), *Quantum Fourier transform over symmetric groups — improved result* — $O(n^4)$-gate $S_n$ QFT used in the runtime estimates.
- Moore--Russell--Sniady (2007), *On the impossibility of a quantum sieve algorithm for graph isomorphism* — includes QFTs over wreath products used for the plethysm instance.
- Harrow (2005), *Applications of coherent classical communication and the Schur transform to quantum information theory* — generalized phase estimation template.
- Christandl--Doran--Walter (2012), *Computing multiplicities of Lie group representations* — classical algorithms for bounded-rank/length multiplicity problems.
- Pak--Panova (2017), *On the complexity of computing Kronecker coefficients* — classical complexity and fixed-length algorithms for Kronecker coefficients.
- Fischer--Ikenmeyer (2020), *The computational complexity of plethysm coefficients* — #P-hardness and fixed-parameter tractability facts for plethysm.
- Fayers (2019), *A note on Kostka numbers* — monotonicity lemma used for the bounded-Specht-dimension Kostka enumeration.

## Cross-links

### Paper notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]

### Trick cards

- [[Multiplicity Estimation by Restricted-Isotypic Fourier Sampling]]
- [[Induced-Representation Multiplicity Sampling by Coset Embedding]]
- [[Dimension-Ratio Triage for Multiplicity Algorithms]]
- [[Kostka Enumeration from a Specht-Dimension Bound]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
