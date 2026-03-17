# Jacobi-Anger Truncation for QSP Simulation

> **Source:** Guang Hao Low and Isaac L. Chuang, *Optimal Hamiltonian Simulation by Quantum Signal Processing*, arXiv:1606.02685, Phys. Rev. Lett. **118**, 010501 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1606.02685) · [PRL](https://doi.org/10.1103/PhysRevLett.118.010501)  
> **Tags:** #trick #QSP #approximation #bessel

## Idea

Express the simulation target in the encoded angle variable using a Jacobi–Anger expansion, truncate the resulting Bessel/Fourier series, and use the truncation degree as the driver of query complexity.

## Why it matters

This is one of the cleanest examples of approximation theory directly dictating quantum query complexity. The simulation algorithm is good because the Bessel tail is well behaved, not because of a mysterious circuit trick.

## Main takeaway

For the sparse-Hamiltonian setting in the original [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] paper, this leads to complexity
$$
O\!\left(td\|H\|_{\max} + \frac{\log(1/\epsilon)}{\log\log(1/\epsilon)}\right),
$$
which matches lower bounds in all parameters. Here $\tau = td\|H\|_{\max}$ is the normalized simulation-time parameter ($d$ the sparsity, $\|H\|_{\max}$ the max entry magnitude).

## Related notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Phased Qubitization Sequence]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
