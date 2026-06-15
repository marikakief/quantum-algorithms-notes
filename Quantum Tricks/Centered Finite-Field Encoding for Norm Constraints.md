# Centered Finite-Field Encoding for Norm Constraints

> **Source:** Chen-Gao-Yuan, arXiv:1802.03856
> **Tags:** #trick #finite-fields #lattice-problems #boolean-encoding

## What it does

Represents $\mathbb F_p$ elements by centred integer lifts before imposing Euclidean norm constraints.

## The trick

For modular equations, the standard representative set $\{0,1,\ldots,p-1\}$ is fine. For norm constraints, it is the wrong geometry: the class $-1\pmod p$ should be short, not have representative $p-1$.

Chen--Gao--Yuan keep the modular equations over $\mathbb F_p$, but for inequalities over $\mathbb C$ use
$$
x_i=\theta_{p-1}(X_i)-\frac{p-1}{2},
$$
so $x_i$ ranges over
$$
\left\{-\frac{p-1}{2},
\ldots,
\frac{p-1}{2}\right\}.
$$
Then a norm condition such as
$$
0<\|X\|_2\le b
$$
can be written as a Boolean polynomial inequality, for instance
$$
\theta_{b^2-1}(G)-
\sum_i\left(\theta_{p-1}(X_i)-\frac{p-1}{2}\right)^2+1=0.
$$
The paper uses the elementary fact that if $\hat X$ lies in the centred box and $v\in(p\mathbb Z)^n\setminus\{0\}$, then
$$
\|\hat X\|_2<\|\hat X+v\|_2,
$$
so the centred lift is the shortest representative of its residue class.

## When to reach for it

Use this when modular variables appear inside integer norm constraints:

- SIS and shortest modular-vector variants;
- finite-field encodings of lattice constraints;
- any reduction where the algebra is modulo $p$ but the objective or inequality is Euclidean.

## Complexity

The encoding uses $O(\log p)$ Boolean variables per field element. The norm polynomial contributes $O(n\log^2p)$ terms after expansion, which is lower order in the SIS bounds when the modular equation system has total sparseness at least $n$.

## Caveat

The displayed symmetric range is cleanest for odd $p$. For even moduli or non-prime rings, one needs a tailored representative system and a separate shortest-representative argument. The trick also handles geometry only after the modular-to-Boolean reduction; it does not improve the condition number of the resulting Boolean system.

## Related notes

- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Truncated Binary Interval Encoding by Theta Polynomials]]
- [[Modular Finite-Field Equations as Boolean Complex Systems]]
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
