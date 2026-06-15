# Review: Term-Weighted Random Hamiltonian Sampling (qDRIFT)

Source note: [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]

## Primary Sources Checked

- Campbell, arXiv:1811.08017: https://arxiv.org/abs/1811.08017

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 8-14: the protocol and scaling are correct.
- Line 14: say "the averaged channel" approximates the target. Individual random circuits are not guaranteed to be close.
- Line 16: "independent of `L`" means independent except through `lambda = sum |h_j|`; for many Hamiltonians `lambda` still grows with the number of terms.
- Lines 22-27: the "when not to use" list is good. It correctly flags locality/commutator structure and high precision.

## Missing or Suggested Additions

- Add one sentence distinguishing qDRIFT from randomized ordering: qDRIFT samples one term per micro-step, while Childs-Ostrander-Su randomizes the order of a full product formula.
