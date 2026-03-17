
> **Source:** András Gilyén, Yuan Su, Guang Hao Low, and Nathan Wiebe, *Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics*, arXiv:1806.01838, STOC 2019  
> **Links:** [arXiv](https://arxiv.org/abs/1806.01838) · [STOC](https://doi.org/10.1145/3313276.3316366)  
> **Tags:** #trick #block-encoding #composition #modularity

## Idea

Treat block-encodings algebraically: if you can block-encode primitive operators, then sums, products, scalings, and controlled combinations of those operators can often be built compositionally.

## Why it matters

This is the abstraction layer that makes modern quantum matrix algorithms modular instead of bespoke. One part of the algorithm designs the encoding; another part applies [[QSVT Meta-Template|QSVT]] or some related transform to the encoded object.

## What to remember

Typical rules of thumb:
- products compose by circuit concatenation,
- linear combinations are handled by [[Linear Combination of Unitaries (LCU)|LCU]]-style control logic,
- normalization factors multiply or add and therefore need to be tracked carefully.

The real enemy is not usually the circuit shape. It is runaway normalization.

## When to use it

Any time the target operator is assembled from simpler pieces: chemistry Hamiltonians, kernel matrices, Sylvester-type constructions, or preconditioned linear-algebra pipelines.

## Related notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[Sandwich LCU for Matrix Equations]]
