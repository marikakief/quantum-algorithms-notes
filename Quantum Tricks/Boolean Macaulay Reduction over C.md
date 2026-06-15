# Boolean Macaulay Reduction over C

> **Source:** Ding--Gheorghiu--Gilyén--Hallgren--Li, arXiv:2111.00405
> **Tags:** #trick #macaulay-matrix #polynomial-systems #boolean-systems #HHL

## What it does

Eliminates non-multilinear monomial columns from a complex Macaulay system by using the Boolean equations $x_i^2-x_i=0$.

## The trick

When the solutions are forced to be Boolean, every monomial is equivalent on the solution set to its multilinear image. Define
$$
\psi\!\left(\prod_i x_i^{a_i}\right)=
\prod_i x_i^{\min(a_i,1)}
$$
and extend $\psi$ linearly over $\mathbb C[x_1,\ldots,x_n]$.

Given quadratic input polynomials
$$
F_1=\{f_1,\ldots,f_m\}\subseteq \mathbb C[x_1,\ldots,x_n]
$$
and Boolean field equations
$$
F_2=\{x_1^2-x_1,\ldots,x_n^2-x_n\},
$$
form a Boolean Macaulay matrix $\widehat B$ whose rows are the coefficient vectors of
$$
\psi(mf),
$$
where $m$ is multilinear and $f\in F_1$. Columns are indexed only by multilinear monomials.

The ordinary Macaulay matrix for $F_1\cup F_2$ can be row-reduced into
$$
\widehat M'
=\begin{pmatrix}
0 & \widehat B\\
I_2 & B_2
\end{pmatrix},
$$
where $I_2$ covers the non-multilinear columns. Thus a solution of the Boolean Macaulay linear system
$$
M_B y=b_B,
\qquad \widehat B=[M_B\mid -b_B],
$$
lifts to a solution of the ordinary Macaulay system.

## When to reach for it

Use this when a polynomial system is over $\mathbb C$ but its intended solutions are Boolean. It is a good fit for algebraic cryptanalysis encodings and for QLSA reductions where non-multilinear monomial variables would bloat the matrix and worsen conditioning.

## Complexity

For quadratic $F_1$, the Boolean Macaulay matrix is
$$
O(m\cdot \#F_1)
$$
sparse, and its nonzero entries are efficiently computable. Its width is at most $2^n-1$ nonconstant multilinear monomials, compared with $(d+1)^n-1$ monomials for a max-degree-$d$ ordinary Macaulay matrix.

## Caveat

The reduction improves the generic lower bound from roughly $(d+1)^{h/2}$ to $2^{h/2}$ for a Hamming-weight-$h$ solution, but it does not make the condition number small. For $h=\Theta(n)$ the QLSA route is still exponentially obstructed.

## Related notes

- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]
- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Hamming-Weight Norm Lower Bounds for Macaulay Solution States]]
- [[Monomial-Subset Coupon Collection for Boolean Solution Readout]]
