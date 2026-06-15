# Truncated QLS Condition Number as a Right-Hand-Side No-Go Test

> **Source:** Ding--Gheorghiu--Gilyén--Hallgren--Li, arXiv:2111.00405
> **Tags:** #trick #HHL #QLSA #condition-number #lower-bound

## What it does

Certifies that a QLSA application is slow for the given right-hand side $b$, even if one tries to truncate away small singular values.

## The trick

For a linear system $Ax=b$, define the truncated QLS condition number
$$
\kappa_b(A):=\frac{\|A\|\,\|A^+b\|}{\|b\|}.
$$
This is bounded above by the usual condition number
$$
\kappa(A)=\|A\|\|A^+\|,
$$
but it is often the sharper quantity for an algorithmic application because QLSA only needs to prepare the state proportional to $A^+b$.

The useful lower-bound argument is simple. Assume $\|A\|\le 1$ and $\|b\|=1$. Let $S$ be the span of right singular vectors with singular value at least $c/\kappa_b(A)$, and let $\Pi_S$ project onto $S$. Then
$$
\|\Pi_S A^+b\|\le \|\Pi_S A^+\|\,\|b\|\le \kappa_b(A)/c,
$$
while
$$
\|A^+b\|=\kappa_b(A).
$$
So the well-conditioned singular subspace contains only a $1/c$ fraction of the target vector's norm. A truncated solver that ignores smaller singular values cannot approximate the target state well unless its truncation scale is still comparable to $\kappa_b(A)$.

## When to reach for it

Use this when analysing a proposed [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] / QLSA application and the authors say “we can use a truncated solver” or “the full condition number may be pessimistic.” It is especially sharp when you can prove a direct lower bound on $\|A^+b\|$ from combinatorial structure, as in Macaulay systems.

## Complexity

The certificate costs whatever it takes to lower-bound $\|A^+b\|$. In [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]], this is done by counting monomial coordinates and by a Gram-matrix shortest-vector argument.

## Caveat

This rules out truncation as an easy fix. It does not rule out a good preconditioner $P$ for which $PA$ has much smaller effective condition number while $P b$ remains efficiently preparable and the new system remains queryable.

## Related notes

- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Hamming-Weight Norm Lower Bounds for Macaulay Solution States]]
