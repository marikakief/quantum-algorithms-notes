
> **Tags:** #trick #qdrift #product-formula
> **Source:** arXiv:1811.08017

## What it does
Uses one fixed micro-time step angle for all sampled terms in [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]], simplifying implementation and analysis of the averaged channel.

## The trick
Set $\tau=t\lambda/N$; every sampled term uses the same magnitude $\tau$, and only the term index and sign change. This is the key simplification relative to deterministic [[Order-Condition Cancellation in Product Formulas|product formulas]] with coefficient-dependent angles, where term $j$ is typically evolved for a time proportional to $h_j t/r$ in each segment.

## When to reach for it
- Hardware where repeated identical-angle rotations are cheaper

## Complexity
Enables simple error bounds via i.i.d. channel composition. The averaged-channel diamond-norm error scales as $O((\lambda t)^2/N)$, so target error $\epsilon$ requires
$$
N = O((\lambda t)^2/\epsilon).
$$

## Caveat
Requires many repetitions when $\lambda t$ is large. The guarantee is for the randomized ensemble channel, not for each sampled circuit to approximate the target unitary deterministically.

## Related Paper Notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
