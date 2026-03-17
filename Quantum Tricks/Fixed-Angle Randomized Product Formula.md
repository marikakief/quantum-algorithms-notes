
> **Tags:** #trick #qdrift #product-formula
> **Source:** arXiv:1811.08017

## What it does
Uses one fixed micro-time step angle for all sampled terms in [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]], simplifying implementation and analysis.

## The trick
Set $\tau=t\lambda/N$; every sampled term uses same $\tau$, only term index changes. This is the key simplification over deterministic [[Order-Condition Cancellation in Product Formulas|product formulas]] where each term may need a different angle.

## When to reach for it
- Hardware where repeated identical-angle rotations are cheaper

## Complexity
Enables simple concentration/error bounds via i.i.d. channel composition.

## Caveat
Requires many repetitions when $\lambda t$ is large.

## Related Paper Notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
