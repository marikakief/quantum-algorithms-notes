# Classical Shadows (Huang-Kueng-Preskill 2020)

---

## Metadata

| Field | Value |
|-------|-------|
| **Authors** | Hsin-Yuan Huang, Richard Kueng, John Preskill |
| **Year** | 2020 |
| **Venue** | *Nature Physics* **16**, 1050–1057 (2020) |
| **arXiv** | [arXiv:2002.08953](https://arxiv.org/abs/2002.08953) |
| **Tags** | #shadow-tomography #classical-shadows #measurement #learning |

---

## Key result

A protocol for estimating many properties of an unknown quantum state $\rho$ using far fewer measurements than full tomography, provided the measurement ensemble is well matched to the observable family. Each copy is measured independently: sample a unitary $U$ from an ensemble, apply it, measure in the computational basis to get $b$, and store the classical pair $(U,b)$.

The measurement channel is

$$
\mathcal M(\rho)
=
\mathbb E_{U}\sum_b
\langle b|U\rho U^\dagger|b\rangle\,
U^\dagger |b\rangle\langle b|U .
$$

When $\mathcal M$ is invertible on the relevant operator subspace, one snapshot is

$$
\hat\rho=\mathcal M^{-1}\!\left(U^\dagger |b\rangle\langle b|U\right),
$$

and it is unbiased: $\mathbb E[\hat\rho]=\rho$. For an observable $O$, the single-shot prediction is $\operatorname{tr}(O\hat\rho)$; median-of-means aggregation gives simultaneous high-probability estimates.

The sample complexity is not simply $O(\log M/\varepsilon^2)$ for arbitrary observables. A standard form is

$$
N =
O\!\left(
\frac{\log(M/\delta)}{\varepsilon^2}
\max_{1\le i\le M}\|O_i\|_{\rm sh}^2
\right),
$$

up to constants and the precise median-of-means convention, where $\|\cdot\|_{\rm sh}$ is the shadow norm induced by the chosen ensemble. The shadow norm is the variance proxy that measures whether the measurement ensemble sees the observable efficiently.

Examples:

- **Local random Clifford / Pauli measurements:** efficient for few-body observables; a weight-$k$ Pauli has shadow-norm scaling like $3^k$.
- **Global Clifford measurements:** efficient for low-rank or global fidelity-type observables; for local observables it can lose the locality advantage.
- **Arbitrary linear observables:** require stating the relevant shadow norm or Hilbert-Schmidt/rank structure before quoting a logarithmic-in-$M$ bound.

---

## Why it matters in context

Compared in [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] as the practically accessible version of shadow tomography for local observables. The trade-off: loses generality (no arbitrary observables with the same bound) but gains implementability.

The readout idea later feeds into [[Interferometric Classical Shadow]], which combines shadow-style randomized measurements with interferometric access to quantities prepared through [[Quantum Oracle Sketching]] in [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]].

---

## Caveats

- The inverse channel can amplify noise or variance; the protocol is useful only when the induced shadow norm is controlled.
- The classical post-processing is efficient for many local, stabilizer, or otherwise structured observables, but not automatically for every observable description.
- The paper estimates properties of states, not full state tomography, unless the requested observable family is itself tomographically complete and the corresponding sample/post-processing costs are paid.
