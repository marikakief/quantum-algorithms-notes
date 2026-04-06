# Local Error Representation for Lattice Product Formulas

> **Source:** Andrew M. Childs and Yuan Su, arXiv:1901.00564
> **Tags:** #trick #hamiltonian-simulation #product-formulas #locality #trotter

## What it does

Rewrites product-formula error as an integral whose integrand is a sum of **local commutators nested inside unitary conjugations**, so the error scaling reflects geometric locality instead of crude norm growth.

## The trick

For a canonical product formula

$$
S(t) = S_s(t)\cdots S_1(t), \qquad S_j(t)=e^{-it b_j B}e^{-it a_j A},
$$

write the error as

$$
S(t)-e^{-iHt}=\int_0^t e^{-i(t-\tau)H} S(\tau) T(\tau)\, d\tau,
$$

where

$$
T(\tau) = -i\sum_{k=1}^s \left[\prod_{l=k-1}^1 S_l^\dagger(\tau)\, (c_kA+d_{k-1}B)\, \prod_{l=1}^{k-1} S_l(\tau)
- \prod_{l=k}^1 S_l^\dagger(\tau)\, (c_kA+d_{k-1}B)\, \prod_{l=1}^{k} S_l(\tau)\right],
$$

with $c_k = \sum_{j=1}^k a_j$ and $d_k = \sum_{j=1}^k b_j$.

Because $T(\tau)$ vanishes to order $p$ for a $p$th-order formula, Taylor-expanding it produces terms built from:

- commutators with $H_{\rm odd}$ or $H_{\rm even}$
- conjugations by exponentials of those local pieces
- only **constant-depth support growth** at each layer

For a 1D nearest-neighbour lattice, each commutator only sees overlapping terms, so the number of nonzero local contributions is $O(n)$, not $O(n^{p+1})$. That is the mechanism behind

$$
\|S_p(t)-e^{-iHt}\| = O(nt^{p+1}).
$$

## When to reach for it

- You need a Trotter bound that respects geometry rather than total term count
- You want to prove linear-in-volume error scaling for local lattice Hamiltonians
- BCH or naïve Taylor expansion is obscuring where locality actually enters

## Why it matters

The representation isolates the real source of error: not just noncommutativity, but **where supports overlap under repeated commutation/conjugation**. That makes the linear-in-$n$ bound transparent.

## Caveat

This is most useful when locality is strong enough that most commutators vanish. For general sparse Hamiltonians without geometric structure, the same representation does not by itself give near-linear scaling.

## Related notes

- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Even-Odd Term Ordering for Lattice Simulation]]
- [[Trotter Commutator-Scaling Bound]]
- [[Product Formulas]]
