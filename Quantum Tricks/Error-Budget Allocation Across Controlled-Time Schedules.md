
> **Tags:** #trick #phase-estimation #resource-allocation
> **Source:** arXiv:1811.08017

## What it does
Allocates simulation error non-uniformly across controlled-time calls (e.g., PE schedules) to minimize total cost.

## The trick
Give larger allowed error to short-time evolutions and tighter error to long-time evolutions according to schedule weighting.

## When to reach for it
- Hamiltonian simulation inside [[Gapped Phase Estimation|phase estimation]] / iterative estimation pipelines

## Complexity
Can reduce total gate budget significantly versus uniform error assignment.

## Caveat
Needs explicit schedule-aware optimization.

## Related Paper Notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
