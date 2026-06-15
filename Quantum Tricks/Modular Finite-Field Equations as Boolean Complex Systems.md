# Modular Finite-Field Equations as Boolean Complex Systems

> **Source:** Chen-Gao-Yuan, arXiv:1802.03856
> **Tags:** #trick #finite-fields #polynomial-systems #boolean-encoding

## What it does

Turns an equation $f=0\pmod p$ into a Boolean polynomial equation over $\mathbb C$ while keeping sparseness polynomially controlled.

## The trick

Start with an MQ system $F\subset\mathbb F_p[X]$. Replace each finite-field variable by a Boolean interval encoding
$$
x_i=\theta_{p-1}(X_i),
$$
where $X_i$ is a block of Boolean variables. After substitution, each equation becomes
$$
f_i^{\mathrm{bit}}\in\mathbb F_p[X_{\mathrm{bit}}].
$$
Interpret coefficients as integers in $\{0,\ldots,p-1\}$. If $f_i^{\mathrm{bit}}$ has $t_i'$ terms, its integer value on a Boolean assignment lies between $0$ and at most $(p-1)t_i'$. The congruence
$$
f_i^{\mathrm{bit}}=0\pmod p
$$
is therefore equivalent to saying that the integer value is some multiple of $p$. Encode that quotient by Boolean variables $U_i$ and impose
$$
P(f_i^{\mathrm{bit}})=f_i^{\mathrm{bit}}-p\theta_{t_i'}(U_i)=0
$$
over $\mathbb C$.

Boolean solutions of the resulting complex system project surjectively to finite-field solutions. The map is not one-to-one because $\theta_{p-1}$ and $\theta_{t_i'}$ can have multiple preimages.

## When to reach for it

Use this when a downstream algorithm works over $\mathbb C$ but the original constraints are modular equations over $\mathbb F_p$, especially when those constraints are already low-degree or can be quadratised.

Typical uses:

- finite-field polynomial solving via [[Macaulay-HHL Pseudo-Solution States]];
- algebraic cryptanalysis over $\mathbb F_p$;
- mixed modular/integer optimization where the modular part must feed a Boolean feasibility oracle.

## Complexity

For MQ systems, Chen--Gao--Yuan give
$$
T_{P(F)}=\widetilde O(T_F\log^2p)
$$
and
$$
N_{P(F)}=O\left(n\log p+\sum_i\log t_i+r\log\log p\right).
$$
For high-degree systems, first quadratise with $Q(F)$, which adds $O(T_FD)$ variables and total sparseness, where
$$
D=n+\sum_i\lfloor\log_2 d_i\rfloor.
$$

## Caveat

The condition number of the final Macaulay system can change badly under this reduction. The trick preserves solutions, but it does not preserve numerical conditioning. That is exactly where the claimed quantum speedup can disappear.

## Related notes

- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Truncated Binary Interval Encoding by Theta Polynomials]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Sparse Parity-to-Complex Polynomial Embedding]]
