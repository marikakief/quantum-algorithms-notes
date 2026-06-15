
> **Source:** András Gilyén, Yuan Su, Guang Hao Low, and Nathan Wiebe, *Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics*, arXiv:1806.01838, STOC 2019  
> **Links:** [arXiv](https://arxiv.org/abs/1806.01838) · [STOC](https://doi.org/10.1145/3313276.3316366)  
> **Tags:** #trick #block-encoding #composition #modularity

## Idea

Treat block-encodings algebraically: if you can block-encode primitive operators, then sums, products, scalings, and controlled combinations of those operators can often be built compositionally.

## Why it matters

This is the abstraction layer that makes modern quantum matrix algorithms modular instead of bespoke. One part of the algorithm designs the encoding; another part applies [[QSVT Meta-Template|QSVT]] or some related transform to the encoded object.

## What to remember

Typical rules of thumb:
- products compose by circuit concatenation: an \((\alpha,a,\epsilon_A)\)-encoding of \(A\) followed by a \((\beta,b,\epsilon_B)\)-encoding of \(B\) gives an \((\alpha\beta,a+b,\alpha\epsilon_B+\beta\epsilon_A+O(\epsilon_A\epsilon_B))\)-encoding of \(AB\), up to projector/ancilla convention details,
- linear combinations are handled by [[Linear Combination of Unitaries (LCU)|LCU]]-style control logic,
- normalization factors multiply or add and therefore need to be tracked carefully.

The real enemy is not usually the circuit shape. It is runaway normalization. You also have to align ancilla projectors, controlled versions, and error budgets; the algebra is clean only when the block-encodings use compatible conventions.

## When to use it

Any time the target operator is assembled from simpler pieces: chemistry Hamiltonians, kernel matrices, Sylvester-type constructions, or preconditioned linear-algebra pipelines.

## Related notes

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[Sandwich LCU for Matrix Equations]]
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — uses composition algebra to build preconditioned operator $I + A^{-1}B$
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — composition of Givens rotation block-encodings with diagonal SELECT is the main structural element; the block-encoding of the full walk step follows from composition rules applied to per-term THC oracles
