# Classical Shadow Channel Inversion

> **Source:** Huang, Kueng, Preskill, arXiv:2002.08953
> **Tags:** #trick #classical-shadows #measurement #tomography

## What it does

Turns one randomized measurement outcome into an unbiased classical estimator $\hat\rho$ of an unknown quantum state $\rho$.

## The trick

Choose a randomized measurement ensemble $\mathcal U$. For each copy of $\rho$, sample $U\sim\mathcal U$, measure $U\rho U^\dagger$ in the computational basis, and record the outcome $b$. The raw post-measurement object is

$$U^\dagger |b\rangle\!\langle b|U.$$

Averaging this procedure defines a channel

$$\mathcal M(\rho)=\mathbb E_{U,b}\left[U^\dagger |b\rangle\!\langle b|U\right].$$

If $\mathcal M$ is invertible, define the single-shot snapshot

$$\hat\rho=\mathcal M^{-1}\!\left(U^\dagger |b\rangle\!\langle b|U\right).$$

Then

$$\mathbb E[\hat\rho]=\rho,$$

so $\operatorname{tr}(O\hat\rho)$ is an unbiased estimator for $\operatorname{tr}(O\rho)$.

For global Clifford measurements,

$$\mathcal M^{-1}(X)=(2^n+1)X-\mathbb I.$$

For local Pauli / single-qubit Clifford shadows,

$$\mathcal M^{-1}=\bigotimes_{j=1}^n\mathcal M_1^{-1},\qquad \mathcal M_1^{-1}(X)=3X-\mathbb I.$$

## When to reach for it

Use this when you need many expectation values from the same experimental dataset, especially when the list of observables may be chosen after the measurements are collected.

## Complexity

One snapshot costs one state preparation, one randomized basis choice, and one computational-basis measurement. The total sample cost depends on the observable family through the [[Shadow Norm as Measurement-Observable Match|shadow norm]].

## Caveat

The estimator $\hat\rho$ is not generally a physical density matrix and may have large variance. Channel inversion must be paired with a good measurement ensemble and usually with [[Median-of-Means Shadow Prediction]].

## Related notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Shadow Norm as Measurement-Observable Match]]
- [[Median-of-Means Shadow Prediction]]
- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]] — shows that the shadow channel inversion primitive needs uploading first when the randomizing circuit would otherwise be noisy physical processing.
