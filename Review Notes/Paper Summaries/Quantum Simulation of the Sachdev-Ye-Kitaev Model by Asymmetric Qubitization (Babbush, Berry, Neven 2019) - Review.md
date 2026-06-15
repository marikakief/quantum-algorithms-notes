# Review: Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush-Berry-Neven 2019)

Source note: [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/1806.02793
- PRA DOI linked in the source note.

## Verdict

Minor issues. The note is detailed and mostly accurate. The main caveat is about the random-circuit state preparation: it samples/realizes a random coupling distribution efficiently, but it is not the same as loading an arbitrary pre-specified SYK disorder instance.

## Line-Anchored Comments

- Lines 21-24: The headline comparison to Garcia-Alvarez et al. is useful. State explicitly that the improvement is for the algorithmic model and target precision considered; it does not include thermal-state preparation or OTOC measurement overheads.

- Lines 35-43: The asymmetric block-encoding explanation is strong. Check the asymmetry overhead formula against the paper's exact normalization; it is a constant for Gaussian weights, but the direction of the ratio matters.

- Lines 47-53: Good description of the self-inverse construction. Add a sentence identifying the signal subspace/projector, since this is what lets QSP apply after the asymmetric embedding.

- Lines 57-63: This is the key nuance. A random orthogonal circuit can generate Gaussian-like amplitudes matching the SYK distribution, but if the problem is to simulate a particular fixed realization of `J_{pqrs}`, one must load those couplings. The efficient random-circuit oracle is best interpreted as efficiently sampling a typical SYK instance.

- Lines 71-74: The SELECT T-count is a useful concrete detail. Clarify whether `N` is the number of Majorana modes or qubits in the resource formula; the note uses `N` as Majorana modes but line 71 says total qubit count `N + ...`, which can be confusing because the physical register is `N/2` fermionic qubits plus ancillas.

- Lines 85-88: The more precise Airy/Bessel asymptotic is good. Mention that this is for the truncation degree in the large-`lambda t` regime, not a separate algorithm.

- Lines 111-115: Comparisons to FeMoCo and solid-state estimates are dated and model-dependent. Preserve the year and assumptions for those numbers.

## Missing or Suggested Additions

- Add a short paragraph explaining what "asymmetric" buys: it avoids expensive exact PREPARE for random Gaussian coefficients by splitting coefficient loading across two states.

- Add a caveat that later SOSSA work changes the best-known SYK scaling, so this note should be read as the 2019 qubitization milestone rather than the final word.

