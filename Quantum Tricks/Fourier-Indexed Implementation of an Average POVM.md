# Fourier-Indexed Implementation of an Average POVM

> **Source:** Harrow--Lin--Montanaro, arXiv:1607.03236
> **Tags:** #trick #POVM #QFT #measurement-implementation #property-testing

## What it does

Implements the uniform average of many projectors as a single projective measurement with an index ancilla.

## The trick

For projectors $\Lambda_1,\ldots,\Lambda_n$, define

$$
\Pi=\sum_{i=0}^{n-1}\Lambda_{i+1}\otimes Q|i\rangle\langle i|Q^{-1},
$$

where $Q$ is the QFT over $\mathbb Z_n$, and

$$
\Delta=I\otimes |0\rangle\langle0|.
$$

Because $Q^{-1}|0\rangle$ is the uniform superposition,

$$
\Delta\Pi\Delta=\left(\frac1n\sum_{j=1}^n\Lambda_j\right)\otimes |0\rangle\langle0|.
$$

This supplies the decomposition needed for the Marriott--Watrous OR test.

## When to reach for it

- Averaging a large family of tests into one coherent measurement.
- Building a POVM implementation from separately available projective measurements.
- Avoiding a classical random choice when amplitude/eigenvalue amplification needs coherence.

## Caveat

This is not automatically time-efficient: implementing controlled access to arbitrary $\Lambda_j$ may cost $O(n)$ without structure.

## Related notes

- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]
- [[Average-Measurement Marriott-Watrous OR Test]]
- [[Quantum Fourier Transform over Finite Abelian Groups]]
