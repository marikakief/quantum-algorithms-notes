
> **Source:** Guang Hao Low and Isaac L. Chuang, *Optimal Hamiltonian Simulation by Quantum Signal Processing*, arXiv:1606.02685, Phys. Rev. Lett. **118**, 010501 (2017); and *Hamiltonian Simulation by Qubitization*, arXiv:1610.06546, Quantum **3**, 163 (2019)  
> **Links:** [QSP arXiv](https://arxiv.org/abs/1606.02685) · [Qubitization arXiv](https://arxiv.org/abs/1610.06546)  
> **Tags:** #trick #QSP #phase-programming #qubitization

## Idea

Interleave iterate applications with carefully chosen ancilla phase shifts so that the resulting sequence implements a desired polynomial transformation of the encoded spectrum.

## Why it matters

This is the programming interface of [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/[[Qubitization Iterate|qubitization]]. Once the iterate has reduced the problem to invariant two-dimensional rotation geometry, the algorithm is basically the phase list.

## When to use it

Use this when you already have the right signal encoded and the remaining task is to implement a spectral transform such as simulation, filtering, sign, thresholding, or polynomial approximation.

## Caveat

The classical phase-synthesis step can be numerically delicate at high degree, so the clean asymptotic story and the practical compilation story are not always equally easy.

## Related notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — applies this sequence for phase estimation on an arbitrary-basis chemistry Hamiltonian; the phase estimation step uses the Jacobi-Anger truncation of the phased iterate
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — applies the phased sequence via QSP to the asymmetric qubitization walk; derives sharp truncation order using Airy-function asymptotics of Bessel functions
