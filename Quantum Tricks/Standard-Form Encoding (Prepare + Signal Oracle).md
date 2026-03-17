
> **Source:** Guang Hao Low and Isaac L. Chuang, *Hamiltonian Simulation by Qubitization*, arXiv:1610.06546, Quantum **3**, 163 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1610.06546) · [Quantum](https://doi.org/10.22331/q-2019-07-12-163)  
> **Tags:** #trick #block-encoding #interface #qubitization

## Idea

Represent the target operator as the projected block of a larger unitary:
$$
H = (\langle G| \otimes I) \, U \, (|G\rangle \otimes I),
$$
where one oracle prepares \(|G\rangle\) and another implements the signal unitary \(U\).

## Why it matters

This is the abstraction layer that lets many different access models feed into the same higher-level simulation machinery. Once an operator is in this form, [[Qubitization Iterate|qubitization]] and later [[Phased Qubitization Sequence|QSP]]/[[QSVT Meta-Template|QSVT]] techniques can act on it uniformly.

## When to use it

Use this as the interface step whenever you want to translate a concrete Hamiltonian-access model into the [[Qubitization Iterate|qubitization]] / [[Block-Encoding Composition Algebra|block-encoding]] world.

## Caveat

The normalization hidden in the encoding is not bookkeeping fluff — it directly determines the final complexity parameter.

## Related notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Qubitization Iterate]]
- [[Linear Combination of Unitaries (LCU)]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
