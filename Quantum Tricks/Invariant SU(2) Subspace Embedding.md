> **Tags:** #trick #spectral-method #geometry #qubitization
> **Source:** Low–Chuang 2019

## What it does
Decouples a global high-dimensional operator transform into independent 2D rotations, one per eigenvalue sector.

## The trick
After [[Qubitization Iterate|qubitization]], Hilbert space decomposes into invariant subspaces
$$\mathcal{H} = \bigoplus_\lambda \mathcal{H}_\lambda, \quad \dim(\mathcal{H}_\lambda)=2,$$
with iterate action as SU(2) rotation by angle $\arccos(\lambda)$ on each $\mathcal{H}_\lambda$.

This converts spectral engineering into single-qubit phase programming.

## When to use
- Deriving functional calculus for encoded operators
- Proving approximation error bounds via scalar polynomial approximation
- Designing filters/projectors/simulators

## Caveat
Requires careful ancilla/projector construction so iterate truly preserves the 2D blocks.

## Related Paper Notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
