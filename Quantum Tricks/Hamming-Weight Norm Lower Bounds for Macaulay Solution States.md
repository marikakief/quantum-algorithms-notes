# Hamming-Weight Norm Lower Bounds for Macaulay Solution States

> **Source:** Ding--Gheorghiu--Gilyén--Hallgren--Li, arXiv:2111.00405
> **Tags:** #trick #macaulay-matrix #condition-number #polynomial-systems #lower-bound

## What it does

Turns the Hamming weight of a Boolean solution into a lower bound on the norm of the Macaulay linear-system solution, hence on QLSA runtime.

## The trick

A Boolean assignment $a\in\{0,1\}^n$ induces a monomial solution vector $y(a)$ whose coordinate for exponent vector $e$ is
$$
y(a)_e=\prod_i a_i^{e_i}.
$$
This coordinate is nonzero exactly when every variable appearing in the monomial is set to $1$ by $a$.

If $a$ has Hamming weight $h$, then counting nonzero monomial coordinates gives:

- for max-degree Macaulay matrices of degree $d$,
  $$
  \|y(a)\|^2=(d+1)^h-1;
  $$
- for total-degree Macaulay matrices of degree $d$,
  $$
  \|y(a)\|^2=\binom{d+h}{h}-1.
  $$

If $My=b$ is the Macaulay linear system and the Boolean solution is unique, then $M^+b=y(a)$. Therefore
$$
\kappa_b(M)=\frac{\|M\|\,\|M^+b\|}{\|b\|}
$$
is at least the square root of the relevant count, up to the normalisation of $M$ and $b$.

For $t$ solutions of the same Hamming weight, the minimum-norm vector in the affine hull can shrink by at most $\sqrt t$, giving the bound
$$
\kappa_b(M)\ge
\sqrt{\frac{(d+1)^h-1}{t}}
$$
for max degree and
$$
\kappa_b(M)\ge
\sqrt{\frac{\binom{d+h}{h}-1}{t}}
$$
for total degree.

## When to reach for it

Use this for any polynomial-solving-to-QLSA proposal where the linear-system variables are monomials evaluated on Boolean assignments. If the solution has many $1$ entries, the monomial vector is automatically large, which is bad news for the condition number.

## Complexity

The argument is combinatorial. It avoids diagonalising the Macaulay matrix. The cost is proving that the QLSA output really is the monomial solution vector or the minimum-norm vector in its affine span.

## Caveat

The cleanest statement is for a unique solution, or for multiple solutions with equal Hamming weight. Mixed-weight solution sets require a more careful affine-hull / Gram-matrix analysis, as in [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]].

## Related notes

- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test]]
- [[Boolean Macaulay Reduction over C]]
