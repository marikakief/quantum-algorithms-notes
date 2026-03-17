# QSVT Meta-Template

> **Source:** András Gilyén, Yuan Su, Guang Hao Low, and Nathan Wiebe, *Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics*, arXiv:1806.01838, STOC 2019  
> **Links:** [arXiv](https://arxiv.org/abs/1806.01838) · [STOC](https://doi.org/10.1145/3313276.3316366)  
> **Tags:** #trick #QSVT #meta-pattern #matrix-algorithms

## Template

1. Build a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of \(A/\alpha\).
2. Identify the scalar function \(f\) you want to apply to singular values or eigenvalues.
3. Approximate \(f\) by a bounded polynomial \(P\) with the right parity. See [[Parity-Aware Polynomial Design]].
4. Compile the phase sequence and run [[QSVT Meta-Template|QSVT]]. See [[Phased Qubitization Sequence]].
5. Account for normalization, postselection, and success amplification if needed. See [[Oblivious Amplitude Amplification (Robust)]].

## Why it matters

This is the master pattern behind a huge fraction of modern quantum linear-algebra algorithms. Once a problem fits this template, the quantum part is often conceptually finished and the real work shifts to encoding and approximation quality.

## What to remember

The complexity driver is usually just:
> degree of the approximating polynomial.

That is the main simplification [[QSVT Meta-Template|QSVT]] brings.

## Related notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Parity-Aware Polynomial Design]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
