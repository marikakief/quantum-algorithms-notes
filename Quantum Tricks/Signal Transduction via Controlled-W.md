
> **Tags:** #trick #QSP #eigenphase #interface
> **Source:** Low–Chuang PRL 2017 (arXiv:1606.02685)

## What it does
Encodes an eigenphase of a large unitary into a controllable single-qubit rotation on an ancilla.

## The trick
Build
$$U_0 = |+\rangle\langle+|\otimes I + |-\rangle\langle-|\otimes W$$
so on eigenstate $|u_\lambda\rangle$ of $W$, ancilla undergoes a rotation parameterized by eigenphase $\theta_\lambda$.

Add single-qubit phase shifts to get phased iterate $U_\phi$.

This is the transducer from high-dimensional spectral data to one-qubit signal processing.

## When to use
Any eigenphase/eigenvalue transform workflow before [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] phase sequencing.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
