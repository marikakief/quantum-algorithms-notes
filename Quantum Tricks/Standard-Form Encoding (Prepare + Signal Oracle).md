
> **Source:** Guang Hao Low and Isaac L. Chuang, *Hamiltonian Simulation by Qubitization*, arXiv:1610.06546, Quantum **3**, 163 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1610.06546) · [Quantum](https://doi.org/10.22331/q-2019-07-12-163)  
> **Tags:** #trick #block-encoding #interface #qubitization

## Idea

Represent the target operator as the projected block of a larger unitary:
$$
\frac{H}{\alpha} = (\langle G| \otimes I) \, U \, (|G\rangle \otimes I),
$$
where one oracle prepares \(|G\rangle\), another implements the signal unitary \(U\), and \(\alpha\ge\|H\|\) is the normalization.

## Why it matters

This is the abstraction layer that lets many different access models feed into the same higher-level simulation machinery. Once an operator is in this form, [[Qubitization Iterate|qubitization]] and later [[Phased Qubitization Sequence|QSP]]/[[QSVT Meta-Template|QSVT]] techniques can act on it uniformly.

Modern block-encoding notation would call this an \((\alpha,a,\epsilon)\)-block-encoding when \(a\) ancillas are used and the projected block approximates \(H/\alpha\) to error \(\epsilon\). In Low-Chuang's standard-form notation this is often written as a tuple \((H,\alpha,U,d)\), where \(d\) tracks the ancilla dimension or access overhead.

For an LCU \(H=\sum_j \beta_j V_j\), the standard PREPARE/SELECT construction gives \(\alpha=\sum_j|\beta_j|\).

## When to use it

Use this as the interface step whenever you want to translate a concrete Hamiltonian-access model into the [[Qubitization Iterate|qubitization]] / [[Block-Encoding Composition Algebra|block-encoding]] world.

## Caveat

The normalization hidden in the encoding is not bookkeeping fluff — it directly determines the final complexity parameter, e.g. qubitized simulation costs \(O(\alpha t+\log(1/\epsilon)/\log\log(1/\epsilon))\) uses of the encoding.

## Related notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Qubitization Iterate]]
- [[Linear Combination of Unitaries (LCU)]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
