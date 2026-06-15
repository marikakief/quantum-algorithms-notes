# Single-Congruence AKS Certificate over a Binomial Modulus

> **Source:** Cheng, arXiv:math/0301179
> **Tags:** #trick #primality-proving #AKS #polynomial-congruence

## What it does

Certifies primality by one AKS-style polynomial congruence modulo $x^r-a$, provided $r$ divides $n-1$ in a controlled way.

## The trick

Let $n$ not be a perfect power. Find a prime $r$ and exponent $\alpha\ge 1$ with

$$
r^\alpha\Vert n-1,
\qquad
r\ge \log^2 n.
$$

Find $1<a<n$ such that

$$
a^{r^\alpha}\equiv 1\pmod n,
\qquad
\gcd(a^{r^{\alpha-1}}-1,n)=1.
$$

Then check the single congruence

$$
(1+x)^n\equiv 1+x^n \pmod{n,x^r-a}.
$$

If it holds, Cheng's theorem says $n$ is prime.

The proof uses the following structure. For a suitable prime divisor $p\mid n$, $a$ is not an $r$-th power in $\mathbb F_p$, so $x^r-a$ is irreducible over $\mathbb F_p$. In the extension $\mathbb F_p(\theta)$, the congruence makes $1+\theta$ and all its $r$ conjugates obey a Frobenius-like relation. Their products form a large subgroup $G_n$ with $|G_n|\ge 2^r$. Automorphism pigeonholing then forces $n$ to be a prime power; the initial perfect-power check makes it prime.

## When to reach for it

- An AKS-type proof needs fewer polynomial congruences for inputs with useful factorisation in $n-1$.
- You can cheaply find an element $a$ with prescribed $r$-power order modulo $n$.
- You want a compact certificate whose verification is dominated by one polynomial modular exponentiation.

## Complexity

The polynomial modular exponentiation costs

$$
\tilde O(r\log^2 n).
$$

With Cheng's choice $r=O(\log^2 n)$, this is $\tilde O(\log^4 n)$.

## Caveat

The modulus degree is $r$, so memory can dominate. Cheng estimates that a $1000$-bit input may create an intermediate polynomial around $2^{30}$ bits. Also, finding a suitable $r$ is the hard part for arbitrary inputs; the paper handles that using [[Medium-Prime-Factor Targeting by One ECPP Round]].

## Related notes

- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Medium-Prime-Factor Targeting by One ECPP Round]]
