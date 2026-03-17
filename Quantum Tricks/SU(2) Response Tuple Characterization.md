
> **Tags:** #trick #QSP-precursor #feasibility #polynomials
> **Source:** Low–Yoder–Chuang PRX 2016 (1603.03996)

## What it does
Determines whether a desired composite-gate response is physically realizable before any phase synthesis.

## The trick
Express composite gate as
$$U(\theta)=A(x)I+iB(x)\sigma_z+iC(y)\sigma_x+iD(y)\sigma_y,$$
with $x=\cos(\theta/2), y=\sin(\theta/2)$.

Feasibility is equivalent to:
- degree bounds (≤ L)
- parity constraints by L
- boundary constraints (e.g. identity at zero angle)
- unitarity polynomial identity (sum-of-squares = 1)

This gives necessary and sufficient realizability conditions.

## When to use
Before optimizing/synthesizing any [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]-like phase sequence.

## Caveat
Ignoring parity early is a common silent failure mode.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
