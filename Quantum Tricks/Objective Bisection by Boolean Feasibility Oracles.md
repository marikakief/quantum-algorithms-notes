# Objective Bisection by Boolean Feasibility Oracles

> **Source:** Chen-Gao-Yuan, arXiv:1802.03856
> **Tags:** #trick #optimization #boolean-feasibility #polynomial-systems

## What it does

Reduces bounded integer-objective minimisation to logarithmically many Boolean feasibility queries.

## The trick

Suppose the feasible set is already encoded as Boolean equations
$$
C(W_{\mathrm{bit}})=0
$$
and the objective satisfies $0\le o(W_{\mathrm{bit}})<u$. Maintain an interval $[\alpha,\mu)$ containing the optimum. Choose
$$
\beta=\lfloor\log_2(\mu-\alpha)\rfloor-1
$$
so that $2^\beta$ is a constant-fraction slice of the interval. To test whether a feasible point has objective in
$$
[\alpha,\alpha+2^\beta),
$$
add Boolean variables $F_0,\ldots,F_{\beta-1}$ and the equation
$$
\delta_{\alpha\beta}(o)=
\alpha+\sum_{j=0}^{\beta-1}F_j2^j-o(W_{\mathrm{bit}})=0.
$$
Then solve
$$
C\cup\{\delta_{\alpha\beta}(o)
\}=0.
$$
If the feasibility call succeeds, update the upper bound to the returned objective value. If it fails, discard the lower slice. The interval shrinks by at least a constant factor per call, so $O(\log u)$ calls suffice.

## When to reach for it

Use this when you have a black-box Boolean or algebraic feasibility solver and need an optimizer, not just a feasible point. It is especially natural for HHL/Macaulay-style solvers, where adding one sparse equation is cheaper than building a full quantum minimum-finding procedure.

## Complexity

If each feasibility system $L_{\alpha\beta}$ has $N_{L_{\alpha\beta}}$ variables, total sparseness $T_{L_{\alpha\beta}}$, and condition number at most $\kappa$, Chen--Gao--Yuan obtain
$$
\widetilde O\!\left(N_{L_{\alpha\beta}}^{2.5}T_{L_{\alpha\beta}}\kappa^2\log(1/\epsilon)\log u\right).
$$
The extra equation contributes only $O(\log u)$ bits and terms.

## Caveat

The method inherits all false-negative and conditioning issues of the feasibility oracle. It also needs a usable upper bound $u$ on the shifted objective. If $u$ is extremely loose, the number of calls is still logarithmic, but the added bits and coefficient sizes grow.

## Related notes

- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Truncated Binary Interval Encoding by Theta Polynomials]]
- [[Modular Finite-Field Equations as Boolean Complex Systems]]
- [[Boolean Solution Extraction by Monomial Measurement]]
