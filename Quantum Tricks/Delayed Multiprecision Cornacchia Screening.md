# Delayed Multiprecision Cornacchia Screening

> **Source:** Morain, arXiv:math/0502097
> **Tags:** #trick #number-theory #ECPP #primality-proving

## What it does

Avoids full multiprecision work in Cornacchia calls that almost certainly fail.

## The trick

Cornacchia's algorithm tests whether a representation such as

$$
4N=U^2+DV^2
$$

exists, after a square root of $-D$ modulo $N$ has been found. Its Euclidean recurrence includes coefficients $w_i$:

$$
r_{i-2}=a_ir_{i-1}+r_i,
\qquad
w_i=w_{i-2}+a_iw_{i-1}.
$$

In the ECPP search, most discriminants fail; Morain estimates success probability around $1/(2h(-D))$. So compute the $w_i$ recurrence first modulo a machine word, e.g.

$$
w_i\equiv w_{i-2}+a_iw_{i-1}\pmod {2^{32}},
$$

and test the final identity only modulo $2^{32}$:

$$
r_{i-1}^2+d w_{i-1}^2\equiv p\pmod {2^{32}}.
$$

Only if this residue test passes do you rerun the recurrence with full multiprecision $w_i$ and check the exact identity.

## When to reach for it

- A deterministic exact arithmetic subroutine is called many times inside a search.
- Most calls fail and failure can be screened by a cheap modular necessary condition.
- The exact recurrence has one part that can be tracked modulo a word without changing the control flow.

## Complexity

The asymptotic Cornacchia cost remains $O(L^{1+\mu})$ for an $L$-bit modulus when a full run is needed. The win is in constants and average cost: almost all failed candidates avoid multiprecision updates for the $w_i$ sequence.

## Caveat

This is a filter, not a proof by itself. A residue hit must be followed by the exact Cornacchia computation; otherwise false positives modulo $2^{32}$ would be accepted.

## Related notes

- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Quadratic-Residue Basis Discriminant Pool for fastECPP]]
