# Sparse Parity-to-Complex Polynomial Embedding

> **Source:** Chen-Gao, arXiv:1712.06239
> **Tags:** #trick #boolean-polynomials #finite-fields #polynomial-systems

## What it does

Converts Boolean equations over $\mathbb F_2$ into sparse complex polynomial equations with the same Boolean solution set.

## The trick

A Boolean parity equation cannot be moved to $\mathbb C$ by simply interpreting $+$ as ordinary addition. For example,
$$
x_1+x_2+1=0\pmod 2
$$
has Boolean solutions, while the same equation over $\mathbb C$ with $x_i^2=x_i$ may not.

For a Boolean polynomial $f=\sum_{i=1}^t m_i$, the exact complex test is
$$
C(f)=\prod_{k=f(0)}^{\lfloor t/2\rfloor}(f-2k),
$$
because a Boolean assignment satisfies $f=0$ over $\mathbb F_2$ exactly when the integer value of $f$ is an allowed even value. This is correct but can be dense.

To keep sparsity:

1. Split a long Boolean equation into 3-sparse equations by introducing auxiliary variables. For $s=3$, a long sum is replaced by a chain of short parity equations using new variables $u_j$.
2. Map each 1-, 2-, or 3-sparse Boolean equation to a complex polynomial by
$$
\widehat C(f)=
\begin{cases}
f,& \#f=1,\\
n_1-n_2,& \#f=2,\ f=n_1+n_2,\\
f-2,& \#f=3,\ f(0)=1,\\
2m_1m_2+2m_1m_3+2m_2m_3-f,& \#f=3,\ f(0)=0.
\end{cases}
$$
3. Add Boolean constraints $y^2-y=0$ for the variables used by the downstream solver.

The resulting complex system has the same projected Boolean zeros as the original system.

## When to reach for it

Use this when an algorithm works over $\mathbb C$ but the input equations are parity constraints over $\mathbb F_2$, especially in algebraic cryptanalysis or Boolean satisfiability reductions.

## Complexity

For an input Boolean system with total sparseness $T_F$, the 3-sparse splitting gives a complex system with at most $T_F$ equations, at most $n+T_F$ variables, and each converted polynomial has sparseness at most $6$.

## Caveat

The embedding preserves Boolean zeros, not arbitrary complex zeros. It must be paired with Boolean constraints and a Boolean-solution extraction method. The auxiliary variables also change the condition number of the Macaulay systems, which is exactly the sensitive parameter in Chen-Gao.

## Related notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Boolean Solution Extraction by Monomial Measurement]]
- [[Macaulay-HHL Pseudo-Solution States]]
