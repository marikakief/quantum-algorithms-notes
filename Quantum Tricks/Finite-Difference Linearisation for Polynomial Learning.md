# Finite-Difference Linearisation for Polynomial Learning

> **Source:** Montanaro, arXiv:1105.3310
> **Tags:** #trick #quantum-learning #finite-differences #polynomial-learning #query-complexity

## What it does

Turns the top-degree part of a multilinear polynomial into affine linear functions that a Bernstein-Vazirani-style routine can learn.

## The trick

For a multilinear polynomial

$$
f(x)=\sum_{T\subseteq[n], |T|\le d}\alpha_T\prod_{i\in T}x_i
$$

over $\mathbb{F}_q$, choose a set $S=\{S_1,\ldots,S_k\}$ and define the iterated finite difference

$$
f_S(x)=\sum_{\beta\in\{0,1\}^k}(-1)^{k-|\beta|}
 f\!\left(x+\sum_{j=1}^k\beta_j e_{S_j}\right).
$$

Equivalently,

$$
f_S=\Delta_{S_1}\Delta_{S_2}\cdots\Delta_{S_k}f.
$$

Set $k=d-1$. Then all monomials not containing $S$ cancel, while monomials containing $S$ lose those variables. Since $f$ has degree at most $d$,

$$
f_S(x)=\alpha_S+\sum_{j\notin S}\alpha_{S\cup\{j\}}x_j.
$$

So each $(d-1)$-fold derivative exposes a slice of the degree-$d$ coefficients as the linear part of an affine function.

## When to reach for it

- Query learning of low-degree multilinear polynomials.
- Reed-Muller codeword identification.
- Any setting where a quantum routine learns linear forms faster than classical sampling.
- Reducing a degree-$d$ learning problem to many degree-1 learning problems.

## Complexity

A query to $f_S$ costs $2^{|S|}$ queries to $f$. For $|S|=d-1$, learning all top-degree coefficients uses

$$
2^{d-1}\binom{n}{d-1}
$$

queries to $f$.

Recursive degree stripping gives total query count

$$
1+\sum_{i=1}^d 2^{i-1}\binom{n}{i-1}.
$$

## Caveat

The exponential factor $2^{d-1}$ is harmless only when $d$ is small. The method also relies on multilinearity; repeated variables and general polynomial interpolation need different handling.

## Related notes

- [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]
- [[Finite-Field Bernstein-Vazirani via Trace Characters]]
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
