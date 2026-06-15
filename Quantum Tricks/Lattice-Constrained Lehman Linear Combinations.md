# Lattice-Constrained Lehman Linear Combinations

> **Source:** Harvey-Hittmeir, arXiv:2105.11105
> **Tags:** #trick #classical-number-theory #factoring #lattices

## What it does

Finds small coefficients $(a,b)$ so that $aq+bp$ is close to a computable estimate while also satisfying a residue constraint tied to $p\bmod m$.

## The trick

Suppose $N=pq$, $p<q$, and we are testing the hypothesis

$$
\sigma_0\le p<\left(1+\frac{1}{m_0}\right)\sigma_0,
\qquad
p\equiv \sigma\pmod m.
$$

The classical Lehman idea is to choose small $a,b$ so that $aq+bp$ is close to a known expression. Harvey and Hittmeir add the modular constraint

$$
-a\frac{N}{\sigma}+b\sigma\equiv 0\pmod m,
$$

which forces

$$
aq+bp\equiv a\frac{N}{\sigma}+b\sigma\pmod {m^2}
$$

whenever $p\equiv\sigma\pmod m$.

The congruence is equivalent to

$$
b\equiv \gamma a\pmod m,
\qquad
\gamma\equiv N/\sigma^2\pmod m,
$$

so the feasible pairs form the rank-2 lattice

$$
L=\langle (1,\gamma),(0,m)\rangle\subseteq \mathbb Z^2.
$$

A linear map sends $(a,b)$ to

$$
c=Na,
\qquad
 d=(-Nm_0)a+(m_0\sigma_0^2)b.
$$

In $(c,d)$-coordinates, the target approximation inequalities become a square. A two-dimensional Lagrange--Gauss reduction then returns a nonzero lattice vector with the desired size bounds in $O(\log^3 N)$ bit operations.

## When to reach for it

- You need coefficients in a Lehman-style expression $aq+bp$.
- You also need those coefficients to obey a modular side condition.
- The side condition is linear in $(a,b)$, so feasible pairs form a low-rank lattice.

## Complexity

For $m_0,\sigma_0,m=O(N)$, the reduction costs $O(\lg^3 N)$ bit operations. The coefficient search is not the bottleneck in the full Harvey--Hittmeir algorithm; the collision stage dominates.

## Caveat

This is a rank-2 trick. Its clean cost bound depends on the congruence cutting out a two-dimensional lattice with an easy basis. Higher-dimensional versions may lose the tight deterministic control that makes the factoring proof work.

## Related notes

- [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]]
- [[Primorial Coprimality Sieve for Deterministic Factoring]]
- [[Product-Tree Bluestein Collision Search Modulo an Unknown Factor]]
