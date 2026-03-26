# Qubitization Iterate

> **Source:** Guang Hao Low and Isaac L. Chuang, *Hamiltonian Simulation by Qubitization*, arXiv:1610.06546, Quantum **3**, 163 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1610.06546) · [Quantum](https://doi.org/10.22331/q-2019-07-12-163)  
> **Tags:** #trick #qubitization #SU2 #quantum-walk

## Idea

Given a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoded / standard-form encoded]] operator, construct a unitary iterate whose action on each eigenvalue is confined to a two-dimensional invariant subspace. In that subspace, the iterate acts as an \(SU(2)\) rotation.

## Why it matters

This is the geometric core of [[Qubitization Iterate|qubitization]]. Once the problem has been reduced to controlled rotations inside invariant two-dimensional sectors, phase-modulated iterate sequences (see [[Phased Qubitization Sequence]]) can implement polynomial spectral transforms efficiently.

## When to use it

Use this whenever you want to implement a function \(f(H)\) and you already have an encoding of \(H\) compatible with [[Qubitization Iterate|qubitization]] / [[Block-Encoding Composition Algebra|block-encoding methods]].

## What to remember

The iterate reduces a hard many-qubit spectral transformation to programmable SU(2) rotation geometry — one 2D invariant subspace per eigenvalue, with the [[Qubitization Iterate|qubitization]] iterate acting as a rotation by $\arccos(\lambda)$ in each.

## Related notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — concrete instantiation of the iterate for arbitrary molecular orbital bases using single factorization and QROAM-based PREPARE
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC-based instantiation achieving $\widetilde{O}(N)$ Toffoli cost per iterate step in arbitrary basis
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — asymmetric generalization where the iterate uses two different PREPARE oracles for bra and ket sides
