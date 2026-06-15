# Poissonized Random-CSP Averaging

> **Source:** Boulebnane--Montanaro, arXiv:2208.06909
> **Tags:** #trick #random-CSP #SAT #QAOA #average-case

## What it does
Chooses the number of random clauses from a Poisson distribution so clause averages exponentiate cleanly.

## The trick
Instead of fixing the number of clauses to $m=\lfloor rn\rfloor$, sample

$$
m\sim \mathrm{Poisson}(rn).
$$

For a quantity that factorises over clauses, the Poisson average turns

$$
\sum_{m\ge0} e^{-rn}\frac{(rn)^m}{m!} A^m
$$

into

$$
\exp(rn(A-1)).
$$

In random $k$-SAT QAOA, this makes the expected success probability depend on an exponential of a polynomial in empirical type frequencies, which can then be attacked by saddle-point methods.

## Why it matters
Fixed-$m$ random CSPs often leave awkward multinomial constraints over clauses. Poissonisation trades exact clause count for independence and a clean exponent. The clause count still concentrates around $rn$, so the exponential scaling in $n$ should match the fixed-ratio ensemble.

## When to reach for it
Use in average-case analyses of random formulas, random hypergraphs, or random constraints where the object being averaged factorises across independently drawn constraints.

## Caveat
Poissonisation slightly changes the ensemble. Usually harmless for leading exponents, but if the result depends sensitively on exact satisfiability probability near threshold, check de-Poissonisation.

## Related notes
- [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]
- [[Type-Count Expansion for QAOA Paths]]
