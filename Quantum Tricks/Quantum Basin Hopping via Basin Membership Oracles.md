# Quantum Basin Hopping via Basin Membership Oracles

> **Source:** Bulger, arXiv:quant-ph/0507193
> **Tags:** #trick #Grover #optimization #local-search

## What it does

Turns a local optimiser into a Grover-search predicate over *basins of attraction*, so the search range is structured by the landscape rather than treated as a flat table.

## The trick

Given an initial point $x_0$, coherently run $L$ steps of local search:

$$
|x_0\rangle |0\rangle^{\otimes L} \mapsto |x_0\rangle |x_1\rangle \cdots |x_L\rangle .
$$

Then evaluate $f(x_L)$ and mark the branch if $f(x_L) < Y$. The Grover predicate is therefore not “is $x_0$ good?” but “does the local-search basin containing $x_0$ descend below threshold $Y$?”

After the phase flip, uncompute the local-search path and reflect about the uniform superposition on starting points. The outer loop updates $Y$ whenever a better basin is found, as in [[Grover Search with Evolving Threshold for Minimum Finding]].

## When to reach for it

- Global optimisation settings where local descent is much better than random sampling.
- Landscapes with large basins around good optima.
- Search problems where a cheap deterministic or coherent relaxation maps many raw candidates to the same local certificate.

## Complexity

If the set of starting points whose local-search endpoint beats $Y$ has size $N_\gamma$ inside a discretised domain of size $N$, the Grover part uses

$$
O\!\left(\sqrt{\frac{N}{N_\gamma}}\right)
$$

iterations, each costing the local-search computation and its inverse. With $L$ local steps, the coherent descent cost is paid $O(L)$ times per Grover iterate.

## Caveat

The coherent local search has to be reversible and sufficiently deterministic, or at least controlled well enough that the threshold oracle does not entangle too much garbage with the search register. If local descent is noisy, discontinuous, or has a huge threshold boundary, use [[Three-Way Grover Analysis for Entangling Oracle Errors]] before trusting the two-dimensional Grover picture.

## Related notes

- [[Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Randomised Iteration Count for Grover Search]]
- [[Grover Search with Evolving Threshold for Minimum Finding]]
