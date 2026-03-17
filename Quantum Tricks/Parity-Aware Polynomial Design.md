# Parity-Aware Polynomial Design

> **Source:** András Gilyén, Yuan Su, Guang Hao Low, and Nathan Wiebe, *Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics*, arXiv:1806.01838, STOC 2019  
> **Links:** [arXiv](https://arxiv.org/abs/1806.01838) · [STOC](https://doi.org/10.1145/3313276.3316366)  
> **Tags:** #trick #QSVT #polynomials #design-constraint

## Idea

In [[QSVT Meta-Template|QSVT]], you do not get to use an arbitrary polynomial. The realizable transform has parity constraints tied to the sequence length and to how the signal is embedded. So the target approximation has to be designed with that parity in mind from the start.

## Why it matters

Parity is an easy place to go wrong. A polynomial can be a perfect approximation numerically and still be the wrong object for [[QSVT Meta-Template|QSVT]] compilation if its parity does not match the allowed transform sector.

## Typical fixes

- split the target into even and odd parts,
- multiply or divide by \(x\) when appropriate,
- redesign the objective on a shifted interval,
- accept a small degree overhead to land in the realizable class.

## Related notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[QSVT Meta-Template]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
