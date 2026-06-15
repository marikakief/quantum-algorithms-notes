# An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes

> **Source:** Lior Eldar and Sean Hallgren, *An efficient quantum algorithm for lattice problems achieving subexponential approximation factor*, arXiv:2201.13450 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2201.13450) · [PDF](https://arxiv.org/pdf/2201.13450)
> **Tags:** #lattice-problems #bounded-distance-decoding #quantum-algorithms #post-quantum-cryptography #phase-estimation

---

## The computational problem

The paper studies **bounded distance decoding** (BDD) on integer lattices, with parameters that separate lattice dimension from the size of the finite quotient.

Given a full-rank integer lattice $L \subseteq \mathbb{Z}^n$, a target $t \in \mathbb{Z}^n$, and a promise that

$$
\operatorname{dist}(t,L) < \epsilon_1 \lambda_1(L), \qquad \epsilon_1 \leq \frac12,
$$

output the unique closest lattice vector. Here

$$
\lambda_1(L)=\min_{v\in L\setminus\{0\}}\|v\|.
$$

The structural restriction is that the lattice is $q$-periodic: $q\mathbb{Z}^n \subseteq L$. Modulo $q$, the lattice becomes a finite abelian subgroup

$$
G=L/q\mathbb{Z}^n \leq \mathbb{Z}_q^n,
$$

with finite group decomposition

$$
G \cong \mathbb{Z}_{q_1}\times\cdots\times\mathbb{Z}_{q_r}.
$$

The new parameter is $r\log q$, where $r$ is the finite-group rank. The paper asks whether BDD can be solved for approximation factors in the subexponential regime when $r\log q$ is much smaller than the ambient dimension scale.

---

## What the paper does

Eldar--Hallgren give a quantum algorithm for BDD on $q$-periodic lattices with finite-group rank $r$, achieving approximation factor roughly

$$
\epsilon_1 = 2^{-\Omega(\sqrt{r\log q})}
$$

in time $\operatorname{poly}(n,\log q)$.

The quantum part is genuinely interesting: it uses a phased superposition of small cubes around the finite lattice subgroup as an approximate eigenvector of shift operators close to lattice points. Phase estimation then produces noisy random inner products with the hidden closest-vector coefficient, turning the worst-case BDD instance into a random lower-dimensional BDD instance that Babai/LLL can solve. My assessment: the technique is clever, but the headline needs the structural parameter $r\log q$; this is not a general efficient algorithm for worst-case lattice problems.

---

## The algorithm / construction

### 1. Work in the finite quotient

Reduce the $q$-periodic lattice $L\subseteq\mathbb{Z}^n$ modulo $q$:

$$
G=L/q\mathbb{Z}^n\leq \mathbb{Z}_q^n.
$$

Choose generators $g_1,\ldots,g_r\in\mathbb{Z}_q^n$ and coefficient space

$$
\widetilde C=[q_1]\times\cdots\times [q_r],
$$

so every $v\in G$ has a unique coefficient vector $c\in\widetilde C$ with $v=Gc$.

BDD in $L$ maps to BDD in $G$ using the modular distance

$$
\|y\|_q=\min_{a\in y+q\mathbb{Z}^n}\|a\|.
$$

After solving the finite problem, the solution is lifted back to the integer lattice.

### 2. Build phased cube states

For a side length $2\sigma$, define a cube state around $y\in\mathbb{Z}_q^n$ by

$$
|C(y)\rangle=\frac{1}{(2\sigma)^{n/2}}\sum_{z\in[\pm\sigma]^n}|y+z\rangle.
$$

For a random label $a\in\mathbb{Z}_q^r$, define the phased cube state

$$
|\psi_a\rangle=\frac{1}{\sqrt{|G|}}\sum_{c\in\widetilde C}\chi_a(c)|C(Gc)\rangle.
$$

Algorithm 15 prepares $|\psi_a\rangle|a\rangle$ efficiently:

1. prepare $|C(0)\rangle|0\rangle$;
2. Fourier transform the coefficient register to get a uniform superposition over $c\in\widetilde C$;
3. add $Gc$ into the cube register;
4. Fourier transform over $\mathbb{Z}_q^r$ and measure the label register, producing a uniformly random $a$.

The earlier obstacle for these lattice superpositions is the uncomputed coefficient register. Here the authors do not try to remove it cleanly. They Fourier-transform it and keep the induced phase.

### 3. Treat phased cube states as approximate shift eigenvectors

Let $U_x|y\rangle=|y+x\rangle$ be the shift by $x\in\mathbb{Z}_q^n$. If $v=Gc\in G$, then

$$
U_v|\psi_a\rangle=\chi_a(-c)|\psi_a\rangle.
$$

If $y$ is merely close to a group element $Gs$, with $\Delta=y-Gs$ and $\|\Delta\|_q\leq \lambda_1(G)/2$, then

$$
\|U_y|\psi_a\rangle-\chi_a(-s)|\psi_a\rangle\|
\leq 4n^{3/4}\sqrt{\frac{\|\Delta\|_q}{\lambda_1(G)}}.
$$

So the target shift $U_t$ for a BDD instance is an approximate eigenoperator on $|\psi_a\rangle$, with eigenvalue encoding the hidden coefficient vector $s$ of the closest group element.

### 4. Run phase estimation despite approximate eigenvectors

The paper proves a small but useful phase-estimation lemma: if

$$
\|U|\psi\rangle-\omega_q^s|\psi\rangle\|\leq \epsilon_{\mathrm{ev}},
$$

then phase estimation returns $O\in\mathbb{Z}_q$ satisfying

$$
\Pr\left[|O-s|_q\leq 129q\epsilon_{\mathrm{ev}}/p_{\mathrm{err}}^2\right]\geq 1-p_{\mathrm{err}}.
$$

For the BDD target $t=Gs+\Delta$, this gives noisy samples of

$$
O\approx (-s)\cdot a \pmod q,
$$

where $a\in\mathbb{Z}_q^r$ is uniformly random.

### 5. Convert noisy inner products into a random BDD instance

Run the sampling routine $m$ times. This gives random rows $a_i\in\mathbb{Z}_q^r$ and noisy outputs $O_i\in\mathbb{Z}_q$. Assemble

$$
\widetilde G=\begin{pmatrix}a_1\\ \vdots\\ a_m\end{pmatrix}\in\mathbb{Z}_q^{m\times r},
\qquad
\widetilde t=(O_1,\ldots,O_m)\in\mathbb{Z}_q^m.
$$

With high probability, $\widetilde G$ defines a random $q$-ary lattice whose shortest vector is long enough, and the same coefficient vector $s$ is the unique closest-vector coefficient for $\widetilde t$.

This is the random self-reduction: worst-case BDD in dimension $n$ becomes random BDD in dimension

$$
m\approx \sqrt{r\log q}
$$

while preserving the hidden coefficient vector.

### 6. Solve the lower-dimensional random BDD instance

For the polynomial-time result, choose $m=\sqrt{r\log q}$ and solve the random instance with Babai's nearest-plane algorithm after LLL-style basis reduction. Random $q$-ary lattices have

$$
\lambda_1(\widetilde G)\gtrsim \sqrt{m}\,q^{1-r/m}
$$

with high probability, so the noisy target lies inside the decoding radius when

$$
\epsilon_1=2^{-\Omega(\sqrt{r\log q})}
$$

up to polynomial factors in $n$.

The paper also gives a tradeoff version: use Schnorr's approximate CVP algorithm instead of Babai on the reduced random instance.

---

## Key results

**Lemma 16: phased cube states are approximate eigenvectors.** For side length

$$
\frac14\frac{\lambda_1}{\sqrt n}\leq 2\sigma\leq \frac12\frac{\lambda_1}{\sqrt n},
$$

and $y=Gs+\Delta$ with $\|\Delta\|_q\leq \lambda_1/2$,

$$
\|U_y|\psi_a\rangle-\chi_a(-s)|\psi_a\rangle\|
\leq 4n^{3/4}\sqrt{\frac{\|\Delta\|_q}{\lambda_1}}.
$$

**Lemma 18: sampling hidden inner products.** If $\operatorname{dist}_q(t,G)\leq \epsilon_1\lambda_1$, then `SampleHIP` returns uniformly random $a\in\mathbb{Z}_q^r$ and $O\in\mathbb{Z}_q$ such that

$$
\Pr\left[|O-(-s)\cdot a|_q
\leq 129\cdot q\cdot \sqrt{\epsilon_1}\cdot 4n^{3/4}p_{\mathrm{err}}^{-2}\right]
\geq 1-p_{\mathrm{err}}.
$$

**Lemma 20: coefficient-preserving random BDD sampling.** `SampleBDD` returns a random subgroup $\widetilde G\leq\mathbb{Z}_q^m$ and target $\widetilde t$ such that, with probability at least $1-p_{\mathrm{err}}$,

$$
\operatorname{dist}_q(\widetilde t,\widetilde G)
\leq
\sqrt{\epsilon_1}\,q^{r/m}\lambda_1(\widetilde G)\,260n^{3/4}m^{2.5}p_{\mathrm{err}}^{-2},
$$

and the closest element of $\widetilde G$ uses the same coefficient vector $s$ as the closest element of $G$.

**Theorem 21.** There is a quantum algorithm running in $\operatorname{poly}(n,\log q)$ time that solves

$$
2^{-\Omega(\sqrt{r\log q})}\text{-BDD}
$$

on $n$-dimensional lattices with periodicity $q$ and finite-group rank $r$.

**Theorem 23.** With tradeoff parameter $2\leq \beta\leq r\log q$, Algorithm Q solves BDD with approximation factor

$$
\epsilon_1=
\frac{
\exp\left(-4\sqrt{\frac{r\log q\log\beta}{\beta}}\right)
}{2^{2m}p_{20}(n,\log q)^2}
$$

in quantum time approximately

$$
\beta^\beta\operatorname{poly}(n,
\log q),
$$

returning the closest vector with probability at least $0.9$.

The displayed formula in the paper is not typographically friendly, but the proof uses the exponent

$$
\exp\left(2\sqrt{\frac{r\log q\log\beta}{\beta}}\right)
$$

from optimizing the reduced dimension

$$
m=\sqrt{\frac{\beta r\log q}{\log\beta}}.
$$

**Corollary 24.** If $r\log q=n\log n$ and $0\leq \epsilon\leq 1/2$, BDD can be solved with factor approximately

$$
\epsilon_1\approx \exp(-4n^\epsilon\log n)
$$

in time

$$
(n^{1-2\epsilon})^{n^{1-2\epsilon}}\operatorname{poly}(n,\log q)
\leq 2^{n^{1-2\epsilon}\log n}\operatorname{poly}(n,\log q).
$$

---

## Comparison with prior work

| Work | Problem / setting | Main method | Approximation / cost |
|---|---|---|---|
| LLL / Babai (1982/1986) | approximate SVP/CVP, BDD via nearest plane | basis reduction | polynomial time for exponential approximation, roughly $2^{-\Theta(n)}$ BDD radius |
| Schnorr (1987/1994) | approximation-time tradeoff | block reduction + enumeration | time $\beta^\beta$ with better approximation than LLL |
| [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] | unique-SVP via dihedral coset problem | quantum reduction to DCP/subset-sum | conceptual bridge; not a direct BDD algorithm |
| Regev (2009) | LWE worst-case reductions | phase-erasing via LWE oracle | uses related lattice superpositions, but for reductions not direct algorithms |
| [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven--Mosca--van de Pol (2013)]] | exact SVP algorithms | Groverised list search | improves sieve/saturation constants; QRACM caveat |
| Ducas--van Woerden (2021) | claim-response note | classical LLL analysis | matches the $r=1$ polynomial-time part and apparently part of the general case |
| **Eldar--Hallgren (2022)** | BDD on $q$-periodic finite-rank lattices | phased cube states + approximate phase estimation + random BDD reduction | $2^{-\Omega(\sqrt{r\log q})}$ in quantum polynomial time; Schnorr tradeoff gives subexponential-time range |

The paper is less about breaking mainstream LWE parameters and more about finding a quantum handle on a neglected parameter regime. The finite-rank view is the cleanest part.

---

## Limits / caveats

1. **Special lattice class.** The algorithm depends on $q$-periodicity and finite-group rank $r$. Every integer lattice is $q$-periodic for $q=\det L$, but that may make $q$ too large for the bounds to help.

2. **Approximation is subexponential in $r\log q$, not automatically in $n$.** The advantage appears when $r\log q$ is small relative to ambient dimension. If $r\log q$ is comparable to $n^2$ or worse, the bound is much less attractive.

3. **Quantum phase estimation only gives noisy linear equations.** The algorithm pays for approximate eigenvector degradation: powers of $U_t$ amplify the shift error, limiting precision.

4. **Classical follow-up matches part of the result.** Ducas--van Woerden showed that LLL already solves at least the $r=1$ polynomial-time claim, and the paper's own Section 7 extends a classical LLL analysis for rectangle-periodic lattices. The remaining quantum edge is mainly the Schnorr tradeoff/general case as framed by the authors.

5. **No direct attack on standard hard lattice assumptions.** The paper does not solve general BDD, LWE, SVP, or uSVP at polynomial approximation factors.

---

## Reusable ideas

1. [[Phased Cube States as Approximate Shift Eigenvectors]] — keep the Fourier-induced phase that normally looks like garbage, and use it as an approximate eigenvalue under near-lattice shifts.

2. [[Approximate-Eigenvector Phase Estimation for Noisy Modular Inner Products]] — run phase estimation on a state that is only an approximate eigenvector, with explicit precision loss from $U^k$ error growth.

3. [[Coefficient-Preserving Random BDD Self-Reduction]] — convert noisy random inner products with the hidden closest-vector coefficient into a lower-dimensional random BDD instance that preserves that coefficient.

---

## References within this paper

- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — earlier quantum connection between lattice problems and dihedral coset states.
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven--Mosca--van de Pol (2013)]] — adjacent quantum-lattice algorithmic context, though based on Groverised sieving rather than phase estimation.
- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes|Chailloux--Loyer (2021)]] — later quantum-walk improvement to lattice sieving, relevant for the wider quantum cryptanalysis setting.
- Regev (2009) — LWE worst-case-to-average-case reduction; cited for using lattice superpositions via oracle-assisted coefficient erasure.
- Aharonov--Regev (2005) — lattice problems in $\mathrm{NP}\cap\mathrm{coNP}$ and related lattice superposition background.
- Babai (1986) — nearest-plane algorithm for approximate CVP/BDD.
- Lenstra--Lenstra--Lovász (1982) — LLL basis reduction.
- Schnorr (1987, 1994) — block reduction hierarchy and approximate CVP tradeoffs used in Algorithm Q.
- Micciancio--Goldwasser (2002) — lattice-problem background and basis reduction lemmas.
- Ducas--van Woerden (2021) — classical response showing LLL already solves part of the polynomial-time claim.
- Chen--Liu--Zhandry (2021) — quantum filtering for average-case lattice variants; cited as another way to handle projections.

---

## Cross-links

### Paper notes

- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]

### Trick cards

- [[Phased Cube States as Approximate Shift Eigenvectors]]
- [[Approximate-Eigenvector Phase Estimation for Noisy Modular Inner Products]]
- [[Coefficient-Preserving Random BDD Self-Reduction]]
- [[QRACM Accounting for Quantum Search over Classical Lists]]
