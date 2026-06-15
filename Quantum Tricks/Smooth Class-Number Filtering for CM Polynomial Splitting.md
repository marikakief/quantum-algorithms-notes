# Smooth Class-Number Filtering for CM Polynomial Splitting

> **Source:** Morain, arXiv:math/0502097
> **Tags:** #trick #CM #ECPP #number-theory

## What it does

Chooses CM discriminants whose class numbers make the class-polynomial root step easier.

## The trick

In ECPP, after a discriminant $-D$ is selected, one computes or retrieves a class polynomial $H_D(X)$ of degree

$$
h=h(-D),
$$

then finds a root modulo $N$ to construct a CM elliptic curve over $\mathbb Z/N\mathbb Z$. A generic split of a degree-$h$ polynomial modulo $N$ is expensive.

Morain's implementation biases the discriminant list toward class numbers with small prime factors. With Galois information about $H_D$, a smooth $h$ lets the program decompose the root-finding problem into a sequence of lower-degree splittings rather than one large degree-$h$ split.

Operationally:

1. Generate candidate discriminants $D$.
2. Estimate or compute $h(-D)$ early.
3. Prefer candidates whose class number is small enough and whose largest prime factor is below a chosen threshold.
4. Use the known Galois structure to factor the modular root problem into smaller pieces.

## When to reach for it

- A search lets you choose among many algebraic parameters with similar correctness value.
- The later cost depends on the factorisation pattern of a degree parameter.
- The algorithm has group-action or Galois structure that can turn smooth degree into staged decomposition.

## Complexity

Morain quotes the modular class-polynomial step as

$$
O((\log L)L^{3+\mu+\nu})
$$

in the fastECPP analysis, with $L=\log N$. Smooth class numbers do not change the headline exponent, but in practice they reduce the root-finding burden by replacing a degree-$h$ split with smaller splits. In the reported timings, the implementation bounded the largest prime factor of $h$ by $30$ for $1000$ and $1500$ decimal digits and by $100$ for $2000$ decimal digits.

## Caveat

Filtering too hard can starve the discriminant search. Morain notes that class numbers tend to be smoother than random integers, but this is still an implementation bias rather than a correctness theorem.

## Related notes

- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Quadratic-Residue Basis Discriminant Pool for fastECPP]]
