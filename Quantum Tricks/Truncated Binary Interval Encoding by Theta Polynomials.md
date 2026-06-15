# Truncated Binary Interval Encoding by Theta Polynomials

> **Source:** Chen-Gao-Yuan, arXiv:1802.03856
> **Tags:** #trick #boolean-encoding #optimization #finite-fields

## What it does

Encodes exactly the integer interval $\{0,1,\ldots,b\}$ using $\lfloor\log_2 b\rfloor+1$ Boolean variables, without producing out-of-range binary values.

## The trick

Let $s=\lfloor\log_2 b\rfloor$ and let $B_0,\ldots,B_s$ be Boolean variables. Define
$$
\theta_b(B)=\sum_{i=0}^{s-1}2^iB_i+(b+1-2^s)B_s.
$$
The lower $s$ bits cover $0,\ldots,2^s-1$ when $B_s=0$. When $B_s=1$, the shifted range is
$$
(b+1-2^s)+\{0,\ldots,2^s-1\},
$$
which covers the upper part of $\{0,\ldots,b\}$. Hence $\theta_b$ is surjective onto the target interval.

Examples:
$$
\theta_6(B)=B_0+2B_1+3B_2,
\qquad
\theta_7(B)=B_0+2B_1+4B_2.
$$
For $b=2^k-1$, this is ordinary binary encoding. For other $b$, the map is not injective; that is a feature, not a bug, when feasibility rather than unique representation is needed.

## When to reach for it

Use this when a Boolean reduction needs an integer variable constrained to a non-power-of-two interval:

- slack variables for inequalities $0\le g\le b$;
- finite-field representatives $x\in\{0,\ldots,p-1\}$;
- bounded integer variables in mixed finite-field/integer optimization;
- interval bits for objective-value search.

## Complexity

Uses $\lfloor\log_2 b\rfloor+1$ Boolean variables and a linear polynomial with the same number of terms. It avoids the degree-$b$ product test
$$
\prod_{i=0}^b(g-i)=0,
$$
which would blow up both degree and size.

## Caveat

The encoding is generally many-to-one. If a later step needs a unique witness, add tie-breaking constraints or use ordinary binary with explicit inequality checks. Also, over $\mathbb F_p$ the paper uses the statement only when $p>b$, so the integer interval is not wrapped modulo $p$.

## Related notes

- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Modular Finite-Field Equations as Boolean Complex Systems]]
- [[Objective Bisection by Boolean Feasibility Oracles]]
- [[Centered Finite-Field Encoding for Norm Constraints]]
