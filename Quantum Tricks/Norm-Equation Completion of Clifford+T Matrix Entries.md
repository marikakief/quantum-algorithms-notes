# Norm-Equation Completion of Clifford+T Matrix Entries

> **Source:** Ross and Selinger, arXiv:1403.2975
> **Tags:** #trick #gate-synthesis #Clifford-T #norm-equation #algebraic-number-theory

## What it does

Turns single-entry approximation into a full Clifford+T unitary by solving one algebraic norm equation.

## The trick

For single-qubit Clifford+T synthesis, first search only for the top-left entry $u\in\mathbb D[\omega]$. If $u$ approximates the desired phase and passes the conjugate-disk test, try to complete it to

$$
U=\begin{pmatrix}u&-t^\dagger\\t&u^\dagger\end{pmatrix}.
$$

Unitarity is equivalent to

$$
u^\dagger u+t^\dagger t=1,
$$

so the unknown $t$ must satisfy

$$
t^\dagger t=\xi,
\qquad
\xi=1-u^\dagger u\in\mathbb D[\sqrt 2].
$$

Now write

$$
\xi^\bullet\xi=\frac{n}{2^\ell},
$$

with $n\in\mathbb Z$ and $\ell$ minimal. Given the prime factorization of $n$, algebraic-number-theoretic routines decide whether the norm equation has a solution in $\mathbb D[\omega]$ and construct $t$ if it does.

This cleanly separates synthesis into:

1. geometric search for promising $u$ values;
2. arithmetic completion by a norm equation;
3. exact synthesis of the resulting matrix.

## When to reach for it

- Exact or approximate synthesis over cyclotomic rings.
- Problems where one matrix entry controls approximation quality but unitarity imposes a hidden completion constraint.
- Compilation routines where a factoring oracle is available, or where trying many candidates until $n$ is easy to factor is acceptable.

## Complexity

The norm-equation step is polynomial in the size of $n$ once a prime factorization of $n$ is known. In Ross--Selinger, if $k$ is the least denominator exponent of $u$, then the generated integer satisfies $n\le 4^k$, and the relevant $k$ values are $O(\log(1/\varepsilon))$.

## Caveat

The candidate $u$ can satisfy the approximation and conjugate-region constraints but still fail the norm equation. Exact optimality requires factoring $n$ for each candidate; without this, the algorithm may skip a candidate that would have completed successfully.

## Related notes

- [[Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) — Paper Notes]]
- [[Convex Grid Normalisation for Algebraic-Integer Candidate Search]]
- [[Clifford Hierarchy for Fault-Tolerant Gate Classification]]
