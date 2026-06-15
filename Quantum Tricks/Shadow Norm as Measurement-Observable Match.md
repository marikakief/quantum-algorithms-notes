# Shadow Norm as Measurement-Observable Match

> **Source:** Huang, Kueng, Preskill, arXiv:2002.08953
> **Tags:** #trick #classical-shadows #measurement-design #tomography

## What it does

Chooses the randomized measurement ensemble by asking which observables it estimates with low variance, rather than by asking whether it reconstructs the whole state well.

## The trick

For a measurement ensemble $\mathcal U$ with average channel $\mathcal M$, define

$$
\|O\|_{\rm shadow}^2
=
\max_{\sigma\;{\rm state}}
\mathbb E_{U\sim\mathcal U}\sum_b
\langle b|U\sigma U^\dagger|b\rangle
\langle b|U\mathcal M^{-1}(O)U^\dagger|b\rangle^2.
$$

This quantity controls the number of measurements needed to estimate $\operatorname{tr}(O\rho)$. The design problem becomes: pick $\mathcal U$ so the target observable family has small shadow norm.

Two basic matches:

- **Global Clifford measurements:** good for observables with small Hilbert--Schmidt norm,

  $$\|O\|_{\rm shadow}^2\le 3\operatorname{tr}(O^2).$$

- **Local Pauli measurements:** good for $k$-local observables,

  $$\|O\|_{\rm shadow}^2\le 4^k\|O\|_\infty^2.$$

The broader lesson: a shadow scheme is not universally good. It is good for a matched class of observables.

## When to reach for it

Use this framing whenever a measurement protocol looks sample-efficient in theory but fails in practice. The right question is usually not “is this a 2-design?” but “what is the shadow norm of the observables I actually care about?”

## Complexity

For $M$ observables, the measurement count scales as

$$O\!\left(\frac{\log(M/\delta)}{\varepsilon^2}\max_i\|O_i\|_{\rm shadow}^2\right).$$

## Caveat

A low sample bound does not guarantee cheap classical post-processing. Clifford shadows can have excellent measurement complexity and still be computationally awkward for some fermionic or many-body observables.

## Related notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Classical Shadow Channel Inversion]]
- [[Median-of-Means Shadow Prediction]]
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]]
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]] — uses noisy shadow weights to show that raw randomized preprocessing can be exponentially worse than uploaded logical preprocessing.
