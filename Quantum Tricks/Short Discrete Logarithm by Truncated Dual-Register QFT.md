# Short Discrete Logarithm by Truncated Dual-Register QFT

> **Source:** Martin Ekerå and Johan Håstad, arXiv:1702.00249
> **Tags:** #trick #discrete-log #QFT #number-theory

## What it does

Samples approximate linear information about a hidden short logarithm $d$ using Fourier registers sized to $m$ rather than to the full group order.

## The trick

Suppose $x=[d]g$ in a cyclic group, with $0<d<2^m$. Choose $\ell\approx m/s$ and prepare

$$
\frac{1}{\sqrt{2^{2\ell+m}}}
\sum_{a=0}^{2^{\ell+m}-1}\sum_{b=0}^{2^\ell-1}|a\rangle|b\rangle|0\rangle .
$$

Compute

$$
[a]g\odot[-b]x=[a-bd]g
$$

and QFT the $a$ register over $2^{\ell+m}$ and the $b$ register over $2^\ell$. A measured pair $(j,k)$ is useful when

$$
\left|\{dj+2^m k\}_{2^{\ell+m}}\right|\le 2^{m-2}.
$$

The small $b$ register asks only for the high-order part of $dj$ modulo $2^{\ell+m}$, which is enough if several samples are later combined.

## When to reach for it

Use it when a discrete logarithm is known a priori to lie in a short interval and the group operation is reversible but full-size Shor exponent registers dominate the quantum cost.

## Complexity

The dominant quantum operation is exponentiation to an exponent of length $\ell+m$ bits for $g$ and $\ell$ bits for $x$. A single run outputs a good pair with probability at least $1/8$ in the Ekerå-Håstad analysis.

## Caveat

The group order must be large enough relative to the sampled rectangle:

$$
r\ge 2^{\ell+m}+2^\ell d.
$$

If wraparound occurs, the clean linear relation behind the good-pair condition can fail.

## Related notes

- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]
- [[Lattice Stitching of Partial Short-Log Samples]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
