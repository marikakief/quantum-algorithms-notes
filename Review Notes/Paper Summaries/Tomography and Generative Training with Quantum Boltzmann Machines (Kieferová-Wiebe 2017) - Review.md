# Review: Tomography and Generative Training with Quantum Boltzmann Machines (Kieferova-Wiebe 2017)

Source note: [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes]]

## Verdict

Major issues. The note is structurally useful and has good caveats about Gibbs preparation, but it states the Golden-Thompson inequality with the wrong direction. Because that inequality is central to the POVM-training part, this needs correction before the note can be trusted as a technical reference.

## Line-Anchored Comments

- Lines 36-52: The Golden-Thompson inequality is written as `Tr[e^{A+B}] >= Tr[e^A e^B]`. For Hermitian `A,B`, the standard Golden-Thompson inequality is `Tr[e^{A+B}] <= Tr[e^A e^B]`. Recheck the objective transformation and rewrite the "lower bound" explanation accordingly.

- Lines 38-50: Once the inequality sign is fixed, spell out which term in the likelihood/objective is being upper- or lower-bounded. The sign of the log-likelihood matters, and the current prose makes it hard to audit.

- Lines 52-58: The key innovation over Amin et al. is plausible, but be careful: non-diagonal POVM data can make quantum terms visible, but only relative to the chosen model family and measurement set.

- Lines 54-62: The relative-entropy gradient is the strongest part of the note. Add that estimating `Tr[rho dH]` still requires measuring the corresponding Hamiltonian terms on data copies; it is not free tomography.

- Lines 64-69: The Gibbs-state preparation paragraph should be less optimistic. The listed algorithms are subroutines under assumptions; generic Gibbs sampling remains a serious bottleneck.

- Lines 79-87: The complexity/lower-bound section needs units. Are queries to a Gibbs oracle, data-state oracle, or state-preparation oracle? Keep those distinct.

- Lines 89-96: The numerical results are very small scale. Keep "exact simulations" and qubit counts next to every qualitative performance claim.

## Missing or Suggested Additions

- Add a corrected derivation of the Golden-Thompson surrogate objective.
- Add a compact table separating objective function, data type, gradient estimator, and bottleneck for the three training methods.
