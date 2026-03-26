# Classical Shadows (Huang-Kueng-Preskill 2020)

> **🚧 STUB** — referenced from Shadow Tomography (Aaronson 2018) notes.

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

A protocol for estimating many properties of an unknown quantum state $\rho$ using far fewer measurements than full tomography. Apply random unitaries from a suitable ensemble (e.g. random Clifford circuits), measure in the computational basis, and classically post-process to form "shadow" estimates of observables.

- Sample complexity $O(\log M / \varepsilon^2)$ for $M$ arbitrary linear observables (matching Aaronson's shadow tomography bound) but with *single-copy* independent measurements — no joint measurements required
- Better than shadow tomography for *local* (few-body) observables; similar or worse for global ones
- Practical: the classical post-processing is efficient for local observables

---

## Why it matters in context

Compared in [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] as the practically accessible version of shadow tomography for local observables. The trade-off: loses generality (no arbitrary observables with the same bound) but gains implementability.

---

## TODO

- [ ] Full derivation of shadow estimator and variance bound
- [ ] Comparison table: classical shadows vs shadow tomography vs randomized measurements
- [ ] Applications to VQE measurement reduction
