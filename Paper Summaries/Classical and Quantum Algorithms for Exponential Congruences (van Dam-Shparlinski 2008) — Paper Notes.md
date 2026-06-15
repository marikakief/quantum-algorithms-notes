# Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes

> **Source:** Wim van Dam and Igor E. Shparlinski, *Classical and Quantum Algorithms for Exponential Congruences*, arXiv:0804.1109, TQC 2008
> **Links:** [arXiv](https://arxiv.org/abs/0804.1109) · [PDF](https://arxiv.org/pdf/0804.1109)
> **Tags:** #quantum-algorithms #finite-fields #discrete-logarithm #hidden-subgroup-problem #exponential-congruences

---

## The computational problem

Fix a finite field $\mathbb F_q$ and nonzero elements $a,b,c,f,g\in\mathbb F_q^*$. Let $s=\operatorname{ord}(f)$ and $t=\operatorname{ord}(g)$.

The task is to decide whether the exponential congruence

$$
a f^x + b g^y = c
$$

has a solution in nonnegative integers $x,y$, and to output one if it exists. Since $f^x$ and $g^y$ are periodic, it is enough to search

$$
0\leq x<s,\qquad 0\leq y<t.
$$

The input size is $O(\log q)$ field bits, but the algorithms in this paper still run in $q^\alpha(\log q)^{O(1)}$ time. So this is not an efficient quantum algorithm in the usual polylogarithmic sense; it is a speedup over the best known finite-field search strategies.

## What the paper does

van Dam and Shparlinski combine finite-field character-sum bounds with Shor discrete logarithms and Grover search. The main worst-case quantum algorithm solves the problem in

$$
q^{3/8}(\log q)^{O(1)},
$$

compared with their deterministic classical bound

$$
q^{9/8}(\log q)^{O(1)}.
$$

My assessment: the result is technically neat rather than field-changing. It gives a natural algebraic problem with a clean cubic quantum speedup over the presented classical method, but it does not get close to the polylogarithmic runtime that would be needed for the semidirect-product HSP application.

## The algorithm / construction

The construction has two layers: a number-theoretic window lemma, then a search algorithm over the short coordinate.

### 1. Count solutions in a rectangular window

For $1\leq r\leq t$, define

$$
N_{a,b,c}(r,s)=\#\{(x,y):0\leq x<s,\ 0\leq y<r,\ a f^x+b g^y=c\}.
$$

The paper proves

$$
N_{a,b,c}(r,s)=\frac{rs}{q-1}+O(q^{1/2}\log q).
$$

The proof uses the multiplicative characters $\mathcal X_k$ of order dividing $k=(q-1)/s$. The character average

$$
\frac1k\sum_{\chi\in\mathcal X_k}\chi(u)
=
\begin{cases}
1,&u\in\langle f\rangle,\\
0,&u\notin\langle f\rangle
\end{cases}
$$

turns the condition $a^{-1}(c-bg^y)\in\langle f\rangle$ into a character sum over $y$. The non-principal characters are bounded by estimates for sums of the form $\sum_{y<r}\chi(c-bg^y)$, giving the $O(q^{1/2}\log q)$ error.

Thus if

$$
r\geq C q^{3/2}s^{-1}\log q
$$

and $r\leq t$, the window $[0,s-1]\times[0,r-1]$ contains a solution.

### 2. Search the short coordinate and solve one discrete log

Assume without loss of generality that $s\geq t$, otherwise swap $(f,x)$ and $(g,y)$. Set

$$
r=\left\lceil C q^{3/2}s^{-1}\log q\right\rceil.
$$

For a candidate $y$, compute

$$
u_y=a^{-1}(c-bg^y).
$$

Then test whether $u_y\in\langle f\rangle$ and, if so, find $x$ such that

$$
f^x=u_y.
$$

Classically this uses generic discrete logarithm in $s^{1/2}(\log q)^{O(1)}$ time. Quantumly it uses [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's discrete-log algorithm]] in $(\log q)^{O(1)}$ time.

There are two cases.

1. **If $r\leq t$:** the character-sum lemma guarantees a solution in the first $r$ values of $y$. Search $y\in\{0,\ldots,r-1\}$.
2. **If $r>t$:** no shortened window is guaranteed, so search all $y\in\{0,\ldots,t-1\}$. If no $y$ works, no solution exists in the full period rectangle.

Quantumly the outer search is [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]], with the discrete-log test as the predicate.

### 3. Improved variants for large-order and typical instances

If the orders $s,t$ are large, the solution count itself gives many marked $y$ values. The paper uses the Boyer--Brassard--Høyer--Tapp version of Grover search with unknown or multiple marked items: if $m$ of $r$ items are marked, find one in $O(\sqrt{r/m})$ predicate calls.

For almost all $c\in\mathbb F_q^*$, an additive-character second-moment argument improves the required window size from roughly $q^{3/2}/s$ to $q^2/s^2$, yielding better typical-case bounds.

## Key results

### Lemma 1: worst-case window count

For $1\leq r\leq t$,

$$
N_{a,b,c}(r,s)=\frac{rs}{q-1}+O(q^{1/2}\log q).
$$

Consequently, if

$$
Cq^{3/2}s^{-1}\log q\leq r\leq t,
$$

then a solution exists with $0\leq x<s$ and $0\leq y<r$.

### Theorem 1: deterministic classical worst case

One can find a solution to $a f^x+b g^y=c$, or decide none exists, in deterministic classical time

$$
q^{9/8}(\log q)^{O(1)}.
$$

The bound includes deterministic factoring of $q-1$ and generic discrete-log computations.

### Theorem 2: deterministic classical typical case

For all but $o(q)$ values of $c\in\mathbb F_q^*$, one can solve or reject the equation in deterministic classical time

$$
q(\log q)^{O(1)}.
$$

### Theorem 3: quantum worst case

One can find a solution, or decide none exists, in quantum time

$$
q^{3/8}(\log q)^{O(1)}.
$$

The algorithm computes $s,t$ and the discrete logs using [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]], and searches over $y$ using [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]].

### Theorem 4: large-order quantum case

There is an absolute constant $C$ such that, if

$$
st>Cq^{3/2}(\log q)^{1/2},
$$

then the problem can be solved in quantum time

$$
q^{1/2}(st)^{-1/4}(\log q)^{O(1)}.
$$

In particular, under this condition the runtime is at most

$$
O(q^{1/8}(\log q)^{O(1)}).
$$

### Theorem 5: quantum typical case

For all but $o(q)$ values of $c\in\mathbb F_q^*$, the problem can be solved in quantum time

$$
q^{1/3}(\log q)^{O(1)}.
$$

### Theorem 6: large-order typical quantum case

For all but $o(q)$ values of $c\in\mathbb F_q^*$, if

$$
st>q^{4/3}(\log q)^{2/3},
$$

then the problem can be solved in quantum time

$$
q^{1/2}(st)^{-1/4}(\log q)^{O(1)}.
$$

## Comparison with prior work

| Problem / method | Runtime | Comment |
|---|---:|---|
| Brute force over $(x,y)$ | $O(st)$, worst-case $O(q^2)$ | Ignores the algebraic structure. |
| Search one coordinate + generic classical discrete log | About $t\sqrt{s}$ | Useful when one order is small, but not balanced. |
| This paper, deterministic classical | $q^{9/8}(\log q)^{O(1)}$ | Uses character sums to shrink the $y$ window. |
| This paper, quantum worst case | $q^{3/8}(\log q)^{O(1)}$ | Replaces generic discrete log by [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]] and coordinate search by [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]]. |
| This paper, quantum large-order case | $q^{1/2}(st)^{-1/4}(\log q)^{O(1)}$ | Uses many marked items once the character-sum estimate gives density. |
| Semidirect-product HSP target | $(\log q)^{O(1)}$ desired | The paper explicitly falls short. |

The HSP motivation comes from [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]], where efficient solution of equations of the form $a f^x+b f^y=c$ would help for $\mathbb Z/q\rtimes\mathbb Z/p$ with $q/p^2=(\log q)^{O(1)}$.

## Limits / caveats

- The headline speedup is polynomial in $q$, but the input size is $\log q$. Runtime $q^{3/8}$ is still exponential in the input length.
- The HSP-relevant special case $f=g$ and $\operatorname{ord}(f)=p\approx\sqrt q$ still only gets a quantum search cost $O^*(q^{1/4})$, not polylogarithmic time.
- The large-order improvements need $st$ above explicit thresholds; they do not cover the hard small-order regimes.
- The typical-case theorems are over the right-hand side $c$. They do not say every cryptographically chosen instance is easy.
- The classical comparison uses rigorous generic discrete-log bounds. In finite fields with special structure, classical subexponential discrete-log algorithms can improve the baseline.

## Reusable ideas

1. [[Character-Sum Windowing for Exponential Congruence Search]] — use multiplicative-character sums to certify that a small rectangular search window contains a solution.
2. [[Grover Search over Discrete-Log Oracles]] — turn a two-variable exponential equation into a one-variable search whose predicate is a quantum discrete-log computation.
3. [[Solution-Density Grover Search from Character-Sum Counts]] — when a number-theoretic estimate gives many marked items, use the marked-item count to reduce Grover cost from $\sqrt r$ to $\sqrt{r/m}$.

## References within this paper

- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon--Childs--van Dam (2005)]] — motivation from semidirect-product HSP and pretty-good-measurement reductions.
- Berndt, Evans, and Williams (1998), *Gauss and Jacobi Sums* — background on character sums.
- Boyer, Brassard, Høyer, and Tapp (1998), *Tight bounds on quantum searching* — the multiple-solution Grover variant used in Theorem 4.
- Crandall and Pomerance (2005), *Prime Numbers: A Computational Perspective* — classical factoring and discrete-log algorithms.
- Dobrowolski and Williams (1992) — character-sum bound used in Lemma 1.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — quantum search.
- Kohel and Shparlinski (2000) — elliptic-curve character-sum analogue suggested in the remarks.
- Lenstra and de Weger (2005) — cryptographic motivation via meaningful hash collisions for public keys.
- Lidl and Niederreiter (1997), *Finite Fields* — finite-field and Gauss-sum background.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — quantum factoring and discrete logarithms.
- Storer (1967), *Cyclotomy and Difference Sets* — cyclotomic-class context.
- Yu (2001) — estimates of character sums with exponential functions.

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]
- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes]]

### Trick cards

- [[Character-Sum Windowing for Exponential Congruence Search]]
- [[Grover Search over Discrete-Log Oracles]]
- [[Solution-Density Grover Search from Character-Sum Counts]]
- [[HSP as Algorithmic Template (Abelian)]]
- [[Pretty Good Measurement for State Discrimination]]
