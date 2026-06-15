# Interferometric Classical Shadow

> **Source:** Zhao, Zlokapa, Neven, Babbush, Preskill, McClean, Huang, arXiv:2604.07639
> **Tags:** #trick #classical-shadows #measurement #readout #overlaps

## What it does

Turns a quantum state $|w\rangle$ into a compact classical object that can later answer many signed-overlap queries $\operatorname{Re}\langle x|w\rangle$ offline.

## The trick

Ordinary measurements of $|x\rangle\langle x|$ on $|w\rangle$ only reveal $|\langle x|w\rangle|^2$, which loses the sign or phase. The paper fixes this by embedding the target state into
$$
|\widetilde w\rangle = \frac{1}{\sqrt 2} (|0\rangle|0\rangle + |1\rangle|w\rangle)
$$
and then applying [[Classical shadows (Huang-Kueng-Preskill 2020)|classical-shadow]] measurements to that augmented state.

For any test state $|x_i\rangle$, define
$$
|x_i^\pm\rangle = \frac{1}{\sqrt 2} (|0\rangle|0\rangle \pm |1\rangle|x_i\rangle)
$$
and the observable
$$
O_i = |x_i^+\rangle\langle x_i^+| - |x_i^-\rangle\langle x_i^-|.
$$
Then
$$
\operatorname{tr}(O_i |\widetilde w\rangle\langle \widetilde w|) = \operatorname{Re}\langle x_i|w\rangle.
$$
So one shadow dataset suffices to estimate many signed overlaps, provided overlaps with stabilizer states can be computed efficiently for the test family.

Conceptually, it is a Hadamard test repackaged so that the expensive quantum part happens once, while the many test queries are answered later by classical post-processing.

## When to reach for it

- When the output of a quantum algorithm is a state $|w\rangle$, but the application needs many classical predictions
- When signs matter, for example LS-SVM classification where one needs $\operatorname{sgn}(\langle x|w\rangle)$
- When the test set is huge or only revealed after the quantum experiment
- When the relevant test states admit efficient overlap computation with stabilizer states

## Complexity

For $m$ target overlaps and additive error $\varepsilon$, the paper gets a shadow-norm-controlled bound of the form
$$
O\!\left(\frac{\log(m/\delta)}{\varepsilon^2}\right)
$$
queries to the controlled state-preparation unitary of $|w\rangle$ for the structured test families it analyzes. The logarithmic dependence on $m$ assumes the corresponding overlap observables have bounded shadow norm and that overlaps between test states and stabilizer shadow states can be computed efficiently. In favourable structured settings the classical prediction time per query is polylogarithmic.

## Caveat

The method is only as good as the structure of the test family. If overlaps with stabilizer states are hard to evaluate, the classical post-processing can become the bottleneck.

Also, it returns the real part of overlaps in the paper's setup. That is exactly what their real-valued learning tasks need. Complex overlaps would require a corresponding imaginary-part observable / phase-shifted variant, or a different readout construction.

## Related notes
- [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]]
- [[Classical shadows (Huang-Kueng-Preskill 2020)]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
