# Review: The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015)

Source note: [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]

## Primary Sources Checked

- McClean, Romero, Babbush, and Aspuru-Guzik, "The theory of variational hybrid quantum-classical algorithms," New Journal of Physics 18, 023023 (2016) / arXiv:1509.04279.

## Verdict

Minor issues. The note is strong and usefully separates what became subfields. The main technical caution is that variance, error suppression, and optimizer benchmarks are all presented in regimes much smaller and cleaner than later VQE practice.

## Line-Anchored Comments

- Lines 7-11: good high-level summary. The "kitchen sink" description is apt, but add the journal publication year somewhere because the filename says 2015 while the NJP paper is 2016.
- Lines 21-30: the VQE recap is correct. Add finite-sampling and calibration caveats to the variational upper-bound statement.
- Lines 36-46: the variance/eigenvalue discussion needs conditions. Small variance certifies proximity to some eigenstate; identifying that eigenstate as the ground state needs energy ordering or gap information.
- Lines 48-60: the variational adiabatic and bang-bang discussion is good. The QAOA connection should be phrased historically with care: Farhi-Goldstone-Gutmann 2014 already existed, while this paper connects related controls to variational hybrid algorithms.
- Lines 62-75: the generalized UCC section is useful. "Universal gate set" should not be read as practical expressibility at shallow depth; the note already says this later, but a pointer here would help.
- Lines 77-85: the critique of variational error suppression is exactly right. The mixed-state variational principle is true, but the noisy optimizer minimizes the noisy objective, not necessarily the ideal objective.
- Lines 87-99: the measurement-reduction section should attach an error bound to term truncation: dropping small coefficients is controlled by the sum of dropped coefficient magnitudes times operator norms, not by smallness term-by-term alone.
- Lines 101-113: the optimizer benchmark caveat is important. The three-orders-of-magnitude claim is for a two-parameter noiseless or lightly noisy toy instance and should not be generalized.

## Missing or Suggested Additions

- Add an explicit distinction between certification of eigenstate quality by variance and preparation of the ground state. VQE can have low variance around an excited state unless the energy window/gap information rules that out.

