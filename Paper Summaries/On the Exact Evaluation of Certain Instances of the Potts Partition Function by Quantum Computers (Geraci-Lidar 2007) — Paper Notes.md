# On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes

> **Source:** Joseph Geraci and Daniel A. Lidar, *On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers*, arXiv:quant-ph/0703023, Communications in Mathematical Physics 279, 735 (2008)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0703023) · [PDF](../sources/zoo-algebraic-number-theoretic/047_quant-ph_0703023.pdf)
> **Tags:** #quantum-algorithms #Potts-model #Tutte-polynomial #coding-theory #Gauss-sums #finite-fields #partition-function

---

## The computational problem

Given a weighted graph $\Gamma=(E,V)$ and prime spin number $q$, compute the $q$-state Potts partition function

$$
Z_\Gamma(y)=\sum_\sigma y^{-|U(\sigma)|}, \qquad y=e^{-J/T},
$$

where $U(\sigma)$ is the set of edges whose endpoints carry equal spins. The paper treats uniform couplings $J$ and hence the fully ferromagnetic and fully anti-ferromagnetic cases at real physical temperature.

The algorithm applies only to graphs in the family $\mathrm{ICCC}_\varepsilon$. For such a graph, with

$$
n=|E|, \qquad k=|E|-|V|+c(\Gamma),
$$

the dual of the graph's cocycle code must be an $[n,k]$ non-degenerate irreducible cyclic code over $\mathbb F_q$, with

$$
n=\frac{q^k-1}{\alpha k^{s(k)}}
$$

and with a digit-sum parameter

$$
\theta_{n,k}=\frac{1}{q-1}\min_{0<j\leq \alpha k^{s(k)}} S'(jn)
$$

satisfying

$$
\varepsilon \leq \frac{q^{\theta_{n,k}-1}}{4\sqrt{q^k}}.
$$

Here $S'(x)$ is the sum of the base-$q$ digits of $x$, and $c(\Gamma)$ is the number of connected components.

## What the paper does

Geraci and Lidar give an exact quantum algorithm for the Potts partition function on a sparse, code-defined graph family. The route is:

$$
\text{graph } \Gamma \to \text{cocycle code} \to \text{dual irreducible cyclic code} \to \text{Gauss sums} \to \text{weight enumerator} \to Z_\Gamma.
$$

The result is narrower than [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|Aharonov-Arad-Eban-Landau (2007)]], but it attacks the physically relevant Potts parameters and returns an exact value, not an additive approximation with a possibly useless scale. My assessment: the graph family is highly engineered, but the coding-theoretic route is genuinely useful; it shows a separate way into statistical-mechanics partition functions, not just another Temperley-Lieb/Jones-polynomial reduction.

## The algorithm / construction

### 1. Convert the graph to a cocycle code

Start with the incidence matrix of $\Gamma$. After row operations, put a cycle matroid matrix representation in the form

$$
\mathrm{CMM}(\Gamma)=[I_{|V|-c(\Gamma)}\mid X].
$$

The row space of $\mathrm{CMM}(\Gamma)$ is the cocycle code $C(\Gamma)$, an $[n, n-k]$ code with $n=|E|$ and $k=|E|-|V|+c(\Gamma)$.

Barg's theorem relates this code to the Potts partition function. If $A(x,y)=\sum_i A_i x^{n-i}y^i$ is the weight enumerator of $C(\Gamma)$, then

$$
Z_\Gamma(y)=y^{-n}q^{c(\Gamma)}A(1,y).
$$

This is the central graph-to-code move; see [[Potts Partition Function via Cocycle-Code Weight Enumerators]].

### 2. Test membership in $\mathrm{ICCC}_\varepsilon$

Form the transpose parity-check matrix for the dual code:

$$
H^T=[-X^T\mid I_k].
$$

Treat the columns of $H^T$ as coordinate vectors in $\mathbb F_{q^k}$. The dual code is equivalent to an irreducible cyclic code exactly when, for a primitive element $g\in\mathbb F_{q^k}^*$, the column elements have discrete logs forming a permuted list of consecutive multiples of

$$
N=\frac{q^k-1}{n},
$$

up to the usual parameter $j$ with $\gcd(n,j)=1$:

$$
(1,g^{Nj},g^{2Nj},\ldots,g^{(n-1)Nj}).
$$

The paper uses [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's discrete-log algorithm]] to compute these logs. The membership test costs

$$
O(|E|\,k^2\log k\log\log k)
$$

quantum time, before the main partition-function computation. This is packaged in [[Discrete-Log Recognition of Irreducible Cyclic Graph Codes]].

### 3. Estimate weights of the irreducible cyclic dual code

For an irreducible cyclic $[n,k]$ code with $q^k-1=nN$, codewords can be represented as

$$
(\operatorname{Tr}(x),\operatorname{Tr}(x\gamma^N),\operatorname{Tr}(x\gamma^{2N}),\ldots,\operatorname{Tr}(x\gamma^{(n-1)N})),
$$

where $x\in\mathbb F_{q^k}$ and $\gamma$ generates $\mathbb F_{q^k}^*$.

The McEliece formula gives the Hamming weight of the codeword indexed by $y\in\mathbb F_{q^k}^*$:

$$
w(y)=\frac{q^k(q-1)}{qN}-\frac{q-1}{qN}\sum_{a=1}^{d-1}\bar\chi(y)^{-a}G_{\mathbb F_{q^k}}(\bar\chi^a,1),
$$

where

$$
d=\gcd\!\left(N,\frac{q^k-1}{q-1}\right),
$$

and $\bar\chi$ is the multiplicative character of order $d$.

Now plug in [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|van Dam-Seroussi's Gauss-sum phase-estimation algorithm]], which estimates

$$
G_{\mathbb F_{q^k}}(\chi,\beta)=\sqrt{q^k}\,e^{i\gamma}
$$

to phase error $\varepsilon$ in

$$
O\!\left(\frac{1}{\varepsilon}(\log(q^k))^2\right)
$$

time.

### 4. Turn approximate Gauss-sum phases into exact integer weights

This is the delicate part. A phase estimate only gives approximate $w(y)$. The paper uses a divisibility theorem for irreducible cyclic-code weights: all weights are divisible by $q^{\theta_{n,k}-1}$. So if the approximation error in $w(y)$ is below

$$
\frac{q^{\theta_{n,k}-1}}{2},
$$

then the exact integer weight is recovered by rounding to the allowed lattice of weights. The sufficient phase-error condition is

$$
\varepsilon < \varepsilon_0=\frac{q^{\theta_{n,k}-1-k/2}}{4}.
$$

This rounding step is the reason the output is exact despite using approximate Gauss-sum phase estimation. See [[Exact Weight Recovery by Divisibility-Gap Rounding]].

### 5. Reduce repeated work using cyclic symmetry

Codewords related by cyclic shift have equal Hamming weight. Since $q^k-1=nN$, there are at most $N$ nonzero weight classes. The paper further notes that $q$-cyclotomic cosets of $\{0,\ldots,N-1\}$ give equal values of the weight expression $S(i)$, so one representative per coset suffices. The number of cosets is

$$
N_C=\sum_{f\mid N}\frac{\varphi(f)}{\operatorname{ord}_q f}\leq N.
$$

The main theorem states the simpler bound using $N=O(k^{s(k)})$ rather than this instance-dependent improvement.

### 6. Recover the cocycle-code enumerator and evaluate $Z_\Gamma$

The Gauss-sum calculation gives the weight enumerator of the dual irreducible cyclic code. The desired Potts formula needs the cocycle code itself, so the paper applies the MacWilliams identity. In the notation of the paper,

$$
A^\perp(1,x)=q^{k(k-n)}\left(1+(q^k-1)x\right)^n A(1,y),
$$

with

$$
x=\frac{1-y}{1+(q^k-1)y}.
$$

Then

$$
Z(x)=x^{-n}q^{c(\Gamma)}A^\perp(1,x),
$$

or equivalently

$$
Z(x(\beta))=q^{c(\Gamma)+k(k-n)}\left[(q^k-1)+x(\beta)^{-1}\right]^n A(1,y(\beta)).
$$

## Key results

### Main theorem

Let $\Gamma=(E,V)$ be a graph with

$$
n=|E|,\qquad k=|E|-|V|+c(\Gamma).
$$

For graphs in $\mathrm{ICCC}_\varepsilon$, a quantum computer returns the exact $q$-state fully ferromagnetic or fully anti-ferromagnetic Potts partition function $Z_\Gamma$. For fixed $\varepsilon$, the stated running time is

$$
O\!\left(\frac{1}{\varepsilon}k^2\max[1,s(k)](\log q)^2\right),
$$

with success probability at least $1-\delta$, where the paper writes

$$
\delta=\left[2\left((q^k-1)^2\varepsilon-2\right)\right]^{-1}.
$$

The text has a minor bracketing typo in this displayed probability expression; the intended dependence is from the usual phase-estimation success bound.

### Direct-sum corollary

If the cycle matroid matrix of $\Gamma$ is the direct sum of the CMMs of two graphs $\Gamma_1,\Gamma_2\in\mathrm{ICCC}_\varepsilon$, then the algorithm evaluates $Z_\Gamma$ with running time equal to the sum of the two subproblem running times. The reason is simply that direct sums of codes multiply weight enumerators.

### Classical comparison in the paper

Assuming membership in $\mathrm{ICCC}_\varepsilon$ is already known, the best classical route cited is via computing zeta functions of curves

$$
C_\alpha: y^q-y=\alpha x^N.
$$

The cited classical cost for all relevant weights is

$$
O\!\left(k^{6s(k)+3+\varepsilon'}\left(q^2\right)^{5+\varepsilon'}\right),
$$

for small real $\varepsilon'$ unrelated to the Gauss-sum phase error. The quantum method is polynomially faster in $k$ for constant $s$, and exponentially faster in $q$ against that route.

## Comparison with prior work

| Work | Problem | Output type | Graph / parameter regime | Main tool |
|---|---:|---:|---|---|
| [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|Aharonov-Jones-Landau (2006)]] | Jones polynomial | additive approximation | braid closures, roots of unity | Temperley-Lieb path model |
| [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|Aharonov-Arad-Eban-Landau (2007)]] | Tutte / Potts | additive approximation | planar graphs, broad complex weights; physical Potts still hard to interpret | generalized Temperley-Lieb algebra |
| Geraci-Lidar (2007) | Potts partition function | exact evaluation | sparse $\mathrm{ICCC}_\varepsilon$ graphs, real physical temperatures, prime $q$ | cocycle codes + Gauss sums |
| [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|van Dam-Seroussi (2002)]] | Gauss-sum phases | phase estimate | finite fields and rings | character states + additive QFT |

This paper is not a general Potts algorithm. It trades breadth for exactness and physical parameters.

## Limits / caveats

- The admissible graphs are special. The condition that the dual cocycle code be irreducible cyclic, plus the $\theta_{n,k}$ divisibility-gap bound, is a serious restriction.
- The paper claims speedup over the best classical methods known to the authors, not an unconditional separation from all classical algorithms.
- $q$ is taken prime for simplicity. Prime powers are natural from coding theory, but the paper does not develop that extension.
- Exactness depends on rounding approximate Gauss-sum phases using a weight divisibility gap. If the gap is too small, the method no longer gives an exact partition function at the stated cost.
- The physical meaning of the graph family is not well characterized. The paper is honest about this: identifying the statistical-mechanics instances represented by these graphs is left open.
- The membership test itself uses quantum discrete logs. Classically, even recognizing the relevant structure appears hard in general.

## Reusable ideas

1. [[Potts Partition Function via Cocycle-Code Weight Enumerators]] — map $Z_\Gamma$ to the weight enumerator of the graph cocycle code, then solve a statistical-mechanics problem through coding theory.
2. [[Exact Weight Recovery by Divisibility-Gap Rounding]] — combine approximate Gauss-sum phase estimation with arithmetic divisibility of irreducible cyclic-code weights to recover exact Hamming weights.
3. [[Discrete-Log Recognition of Irreducible Cyclic Graph Codes]] — recognize when a graph's dual cocycle code is irreducible cyclic by checking discrete logs of parity-check columns.
4. [[Cyclotomic-Coset Compression for Cyclic-Code Weight Spectra]] — evaluate one representative per $q$-cyclotomic coset because the trace/Gauss-sum expression is invariant under Frobenius powers.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — discrete-log subroutine for recognizing irreducible cyclic-code structure.
- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|van Dam-Seroussi (2002)]] — Gauss-sum phase-estimation primitive used to compute cyclic-code weights.
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|Aharonov-Jones-Landau (2006)]] — prior Jones-polynomial approximation route into Potts/Tutte problems at roots of unity.
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|Aharonov-Arad-Eban-Landau (2007)]] — contemporaneous additive approximation algorithm for the Potts model and Tutte plane.
- Daniel A. Lidar (2004) — earlier discussion of quantum computational complexity of Ising spin-glass partition functions and knot invariants.
- Barg (2002) — Potts partition function as a weight enumerator of a graph cocycle code.
- Baumert and McEliece (1972) / McEliece weight formula — weights of irreducible cyclic codes via Gauss sums.
- Aubry and Langevin (2005) — divisibility theorem for weights of irreducible cyclic codes used for exact rounding.
- Denef and Vercauteren (2004) — classical zeta-function route used for comparison.
- Kedlaya (2004) — quantum algorithm for zeta functions of curves, suggested as an alternate quantum route.
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] — quantum counting can speed up the tally of weight multiplicities, though it does not dominate the total cost.

## Cross-links

### Paper notes

- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes]]
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]

### Trick cards

- [[Potts Partition Function via Cocycle-Code Weight Enumerators]]
- [[Exact Weight Recovery by Divisibility-Gap Rounding]]
- [[Discrete-Log Recognition of Irreducible Cyclic Graph Codes]]
- [[Cyclotomic-Coset Compression for Cyclic-Code Weight Spectra]]
- [[Gauss-Sum Phase from Multiplicative-Character Fourier Eigenstates]]
