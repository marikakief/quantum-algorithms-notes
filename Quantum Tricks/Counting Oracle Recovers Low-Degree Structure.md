# Counting Oracle Recovers Low-Degree Structure

> **Source:** van Dam, Mosca, Vazirani, arXiv:quant-ph/0206003
> **Tags:** #trick #query-complexity #oracle #3SAT #adiabatic

## What it does
Shows that a counting oracle (reporting how many constraints an assignment violates) reveals enough structure to reconstruct the entire objective function from polynomially many queries, bypassing quantum search lower bounds.

## The trick

For a $k$-CNF formula $\Phi$ on $n$ variables, define $F_\Phi(b) = $ number of unsatisfied clauses under assignment $b$. Each clause involves at most $k$ variables, so $F_\Phi$ is a degree-$k$ polynomial over the integers in the bits $b_1, \ldots, b_n$:

$$F_\Phi(b) = F_\Phi(0^n) + \sum_{i \in I} Y_i + \sum_{i < j \in I} Y_{ij} + \sum_{i < j < k \in I} Y_{ijk}$$

where $I = \{i : b_i = 1\}$. The coefficients $Y_i, Y_{ij}, Y_{ijk}$ are determined by evaluating $F_\Phi$ on the $O(n^k)$ inputs of Hamming weight $\leq k$.

**For 3SAT:** $O(n^3)$ queries to the counting oracle determine $F_\Phi$ on all $2^n$ inputs. This is a standard Möbius inversion / inclusion-exclusion argument.

**Consequence:** The exponential quantum search lower bound ($\Omega(\sqrt{N})$ for decision oracles, $\Omega(N^{1/6})$ for relational problems) doesn't apply to the adiabatic 3SAT setup, because its oracle is far more informative than a bare "is this the solution?" oracle. Query complexity techniques cannot rule out efficient adiabatic algorithms for structured problems.

## When to reach for it

- Arguing that oracle lower bounds don't apply to a particular algorithm because its oracle reveals structural information
- Analysing the power of different oracle types (counting vs. decision vs. evaluation)
- Understanding why local-checkability (Cook-Levin) breaks relativisation: the counting oracle encodes the local structure that makes SAT reductions work

## Complexity

$O(n^k)$ queries for $k$-local constraint satisfaction. For 3SAT: $O(n^3)$.

## Caveat

This is purely an information-theoretic result. Knowing $F_\Phi$ on all inputs doesn't help you solve SAT efficiently — you still need to find a zero of a polynomial, which is presumably hard. The result says only that *oracle techniques* can't prove hardness for this model.

## Related notes
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
