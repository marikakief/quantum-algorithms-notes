# Extended Prange Attack over Extension Fields

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #classical-attack #coding-theory #finite-fields #OPI #DQI

## What it does

Strengthens Prange-style attacks on OPI over extension fields by spending base-field affine constraints coordinate by coordinate.

## The trick

For OPI over $\mathbb{F}_{p^b}$, identify field elements with vectors in $\mathbb{F}_p^b$ through a trace-basis map $\phi$. Instead of forcing a coordinate into a specific target value, spend $s$ base-field linear constraints to force it into an affine subspace of codimension $s$.

For each codimension $s$, define

$$
P[s] =
\max_{A \in \mathrm{Aff},\ \mathrm{codim}(A)=s}
\frac{|A\cap \phi(F)|}{|A|}.
$$

The attacker has a budget of $nb$ base-field constraints. An allocation strategy chooses codimensions $s_i$ for coordinates with $\sum_i s_i \le nb$, giving Bernoulli success variables $X_i \sim \mathrm{Ber}(P[s_i])$.

Two optimization views:

1. Maximize expected hits with a linear program over a distribution $p_s$.
2. Maximize $\Pr[\sum_i X_i \ge t]$ with a knapsack-style dynamic program.

## When to reach for it

- Checking whether a proposed extension-field OPI instance is harder than Prange suggests.
- Designing target sets $F$ for DQI+OPI.
- Any finite-field search problem where constraints over $\mathbb{F}_{p^b}$ can be partially enforced over $\mathbb{F}_p$.

## Complexity

The attack's per-trial cost is similar in spirit to Prange: solve the enforced linear constraints, then test the remaining objective. The paper focuses on trial counts. For Twisted Bent Target OPI, the XP lower bound is at least $\exp(0.02m)$ trials for a rate $R \approx 1/10$ target.

## Caveat

XP is an attack, not a hardness proof. Bounding XP leaves open other algebraic attacks that exploit low-degree polynomial structure. The paper explicitly treats the classical side as "best known algorithms".

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[Twisted Bent Target Sets for OPI Hardness]]
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
