
> **Tags:** #trick #phase-synthesis #QSP-precursor #compiler
> **Source:** Low–Yoder–Chuang PRX 2016

## What it does
Compiles realizable polynomial response functions into a shortest sequence of phase angles.

## The trick
Given degree-L realizable tuple, recursively peel off one primitive rotation at a time:
1. Compute phase-sum coefficient representation.
2. Choose terminal phase to cancel highest-order component in residual.
3. Reduce to degree L-1 instance.
4. Repeat until base case.

Outputs exactly L phases (minimal length).

## Why it's important
Replaces exponential search over phase space with polynomial-time deterministic compilation.

## When to use
Any [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/composite-pulse pipeline after polynomial design/completion.

## Caveat
Stable numerics require careful coefficient conditioning for high L.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
