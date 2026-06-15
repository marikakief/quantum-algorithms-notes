# Desynchronised Reversible Euclidean Algorithm

> **Source:** Proos and Zalka, arXiv:quant-ph/0301141
> **Tags:** #trick #reversible-computation #quantum-arithmetic #Euclidean-algorithm

## What it does

Runs a variable-time Euclidean algorithm on a superposition by letting each branch carry flags for its current micro-operation.

## The trick

The extended Euclidean algorithm has input-dependent quotients and an input-dependent number of iterations. A quantum circuit cannot simply let different basis states halt at different physical times unless the whole process remains reversible.

Proos and Zalka decompose each Euclidean iteration into five reversible operation types:

1. grow a shift counter until $2^iB>A$;
2. compute quotient bits by high-to-low conditional subtraction;
3. update the coefficient register using those quotient bits;
4. uncompute the shift counter;
5. swap the Euclidean pairs so the larger remainder is first.

Each basis branch stores a small control register $c$ saying which operation type it needs, plus a flag $f$ saying whether the current run of that operation has ended. The global circuit cycles through the operation types. Operation $o_i$ acts only on branches with $c=i$; after a branch finishes a run, an “advance counter” step updates $c$.

Schematically,

$$
o_i': |x,f,c\rangle \mapsto |o_i(x), f\oplus \mathrm{first}\oplus \mathrm{last}, c\rangle
$$

when $c=i$, and otherwise does nothing. Then

$$
\mathrm{advance}: |x,f,c\rangle \mapsto |x,f,c+f\rangle.
$$

Completed branches do not stop irreversibly. They increment a small completion counter during later cycles, leaving enough information to reverse the computation.

## When to reach for it

- Reversible arithmetic with data-dependent loop counts.
- Quantum algorithms where a classical subroutine is fast because it exploits small quotients or short carries.
- Situations where padding every branch to the worst local operation would lose an asymptotic factor.

## Complexity

For modular inversion modulo an $n$-bit prime $p$, Proos--Zalka prove a cycle bound

$$
t(p,x)=r+4\sum_{i=1}^{r}\lfloor \log_2 q_i\rfloor \leq 4.5\log_2 p,
$$

where $q_i$ are Euclidean quotients. Each cycle costs $O(n)$ reversible addition/comparison work, giving $O(n^2)$ time and $O(n)$ space.

## Caveat

The method pays in control complexity and a small amount of reversible “halting” garbage. In Proos--Zalka this is acceptable because the inversion routine is later run backward inside the division construction.

## Related notes

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]
- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Fixed-Round Reversible Montgomery-Kaliski Inversion]]
- [[One-Bit Branch Logging for Reversible Binary GCD]]
- [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
