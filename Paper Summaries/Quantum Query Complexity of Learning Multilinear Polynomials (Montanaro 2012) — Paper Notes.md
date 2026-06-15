# Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes

> **Source:** Ashley Montanaro, *The quantum query complexity of learning multilinear polynomials*, Information Processing Letters 112(11):438–442 (2012); arXiv:1105.3310
> **Links:** [arXiv](https://arxiv.org/abs/1105.3310) · [PDF](https://arxiv.org/pdf/1105.3310)
> **Tags:** #query-complexity #quantum-learning #finite-fields #Reed-Muller #Bernstein-Vazirani

---

## The computational problem

Given oracle access to an unknown degree-$d$ multilinear polynomial

$$
f(x)=\sum_{S\subseteq[n], |S|\le d}\alpha_S\prod_{i\in S}x_i
$$

over a finite field $\mathbb{F}_q$, learn all coefficients $\alpha_S$ using as few queries to $f$ as possible. The task is identifying the whole polynomial, not merely evaluating it at one point.

The oracle is the standard addition oracle

$$
O_f|x\rangle|y\rangle=|x\rangle|y+f(x)\rangle,
$$

where $x\in\mathbb{F}_q^n$ and $y\in\mathbb{F}_q$.

For $q=2$, this is exactly learning a codeword from the binary Reed-Muller code of order $d$, because every Boolean function has a multilinear representation.

## What the paper does

Montanaro generalises [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani]] from linear functions to constant-degree multilinear polynomials over finite fields. For fixed $d$, the main result is tight:

$$
Q(f)=\Theta(n^{d-1})
$$

queries, while classical bounded-error learning needs $\Omega(n^d)$ queries.

My assessment: this is a neat query-complexity note rather than a broad algorithmic framework. The real trick is to use $(d-1)$-fold finite differences to turn the degree-$d$ part into many linear functions, then apply a finite-field Bernstein-Vazirani routine.

## The algorithm / construction

### Classical baseline

A direct classical learner queries all Boolean-support points of Hamming weight at most $d$:

$$
1+n+\binom{n}{2}+\cdots+\binom{n}{d}=O(n^d)
$$

for constant $d$.

This is also classically optimal up to constants, by an information count: there are

$$
q^{1+n+\binom n2+\cdots+\binom nd}
$$

possible degree-$d$ multilinear polynomials, and each classical query returns one field element.

### Quantum lower bound

A quantum query can be viewed as one round of communication with the oracle. In each round the algorithm sends $(x,y)$, using $(n+1)\log_2 q$ qubits, the oracle applies $y\mapsto y+f(x)$, and the registers return.

If $f$ is uniformly random from the polynomial class, Holevo's theorem bounds the mutual information after $r$ queries by

$$
I(X:Y)\le 2r(n+1)\log_2 q.
$$

Fano's inequality then implies that bounded-error identification needs

$$
r=\Omega(n^{d-1}).
$$

See [[Holevo-Fano Query Lower Bound for Function Learning]].

### Quantum upper bound: differentiate until linear

For a subset $S=\{S_1,\ldots,S_k\}\subseteq[n]$, define

$$
f_S(x)=\sum_{\beta_1,\ldots,\beta_k\in\{0,1\}}(-1)^{k-\sum_i\beta_i}
 f\!\left(x+\sum_{j=1}^k\beta_j e_{S_j}\right).
$$

This is the iterated finite difference

$$
f_S=(\Delta_{S_1}\Delta_{S_2}\cdots\Delta_{S_k}f).
$$

A query to $f_S$ costs $2^k$ queries to $f$.

Now take $|S|=d-1$. For a degree-$d$ multilinear polynomial,

$$
f_S(x)=\alpha_S+\sum_{k\notin S}\alpha_{S\cup\{k\}}x_k.
$$

So $f_S$ is an affine linear function. One finite-field Bernstein-Vazirani query learns its linear coefficients, hence all $\alpha_{S\cup\{k\}}$ for $k\notin S$.

Algorithm for the degree-$d$ component:

1. For each $S\subseteq[n]$ with $|S|=d-1$:
   - implement one query to $f_S$ using $2^{d-1}$ queries to $f$;
   - run the finite-field Bernstein-Vazirani procedure to learn the linear part of $f_S$;
   - record the coefficients $\alpha_{S\cup\{k\}}$.
2. Subtract the recovered degree-$d$ component from $f$ inside future oracle calls.
3. Recurse on degree $d-1$.
4. Query $f(0^n)$ once to get the constant term.

The reusable move is [[Finite-Difference Linearisation for Polynomial Learning]].

## Key results

### Proposition 1: quantum lower bound

Any bounded-error quantum query algorithm that learns an unknown degree-$d$ multilinear polynomial over $\mathbb{F}_q$ must make

$$
\Omega(n^{d-1})
$$

queries.

### Theorem 2: exact quantum upper bound

There is an exact quantum algorithm that learns $f$ using

$$
1+\sum_{i=1}^d 2^{i-1}\binom{n}{i-1}
$$

queries to $f$.

For constant $d$, this is $O(n^{d-1})$.

### Lemma 3: finite-field Bernstein-Vazirani

Let $f:\mathbb{F}_q^n\to\mathbb{F}_q$ be linear and $g(x)=f(x)+\beta$. Then $f$ can be determined exactly using one query to $g$.

The algorithm uses the quantum Fourier transform over $\mathbb{F}_q$, phase kickback by additive characters, and an inverse QFT. The trace-character formulation is needed when representing non-prime fields through traces to the prime field. The additive constant $\beta$ becomes a global phase.

### Lemma 4: derivative characterisation

For $|S|=d-1$,

$$
f_S(x)=\alpha_S+\sum_{k\notin S}\alpha_{S\cup\{k\}}x_k.
$$

All degree-$d$ coefficients containing $S$ appear as linear coefficients of $f_S$.

## Comparison with prior work

| Work | Result | Relationship |
|---|---|---|
| [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] | learns linear Boolean functions with one query | $d=1$, $q=2$ case |
| de Beaudrap-Cleve-Watrous (2002) | finite-field Fourier query techniques | source of the QFT-over-$\mathbb{F}_q$ machinery |
| van Dam-Hallgren-Ip (2006) | finite-field hidden shift algorithms | independent finite-field QFT formulation |
| Rötteler (2009) | bounded-error learning of quadratic Boolean polynomials in $O(n)$ queries | Montanaro gives an exact all-$d$, all-$q$ version |
| Kaufman-Ron (2006) | classical low-degree polynomial testing | uses related derivative functions |
| Farhi et al. / Servedio-Gortler | lower bounds for Boolean learning | overlap with the $q=2$ lower-bound story |

## Limits / caveats

- The $O(n^{d-1})$ upper bound is for constant $d$. If $d$ grows with $n$, the binomial sum and the $2^{d-1}$ derivative cost matter.
- The result is query complexity. It does not price the circuit cost of arithmetic over $\mathbb{F}_q$ or finite-field QFTs.
- The learner assumes multilinearity. General polynomial interpolation over finite fields is a different problem; later work gives quantum algorithms for that setting.
- For $q=2$, every function is multilinear, but the promise of degree $d$ is still strong: it says the Boolean function lies in Reed-Muller order $d$.
- For $d=2$, the algorithm takes one finite difference in each coordinate direction; each derivative is affine, and one finite-field Bernstein-Vazirani query learns a row of the quadratic coefficient matrix.

## Reusable ideas

1. [[Finite-Difference Linearisation for Polynomial Learning]] — use $(d-1)$ discrete derivatives to turn the top-degree component of a multilinear polynomial into affine linear functions.
2. [[Finite-Field Bernstein-Vazirani via Trace Characters]] — use phase kickback and the QFT over $\mathbb{F}_q$ to learn a linear form over a finite field with one query.
3. [[Holevo-Fano Query Lower Bound for Function Learning]] — turn a query algorithm for exact identification into a communication process and bound the information gained per query.

## References within this paper

- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — one-query learning of linear Boolean functions.
- de Beaudrap-Cleve-Watrous (2002) — finite-field Fourier algorithms and query separations.
- Cleve-van Dam-Nielsen-Tapp (1998) — communication complexity and Holevo-style information arguments.
- van Dam-Hallgren-Ip (2006) — finite-field QFT and hidden shifts.
- Farhi-Goldstone-Gutmann-Sipser (1999) — function distinguishability with $k$ quantum queries.
- Høyer-Špalek (2005) — survey of quantum query lower bounds.
- Jordan (2008) — quantum estimation of quadratic forms over the reals.
- Kaufman-Ron (2006) — testing low-degree polynomials over finite fields.
- Rötteler (2009) — hidden shift algorithms for quadratics and functions of large Gowers norm.
- Servedio-Gortler (2001) — quantum vs. classical learnability.

## Cross-links

### Paper notes

- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]

### Trick cards

- [[Finite-Difference Linearisation for Polynomial Learning]]
- [[Finite-Field Bernstein-Vazirani via Trace Characters]]
- [[Holevo-Fano Query Lower Bound for Function Learning]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
