# Review: Projected Quantum Kernel via Local Observables

Source note: [[Projected Quantum Kernel via Local Observables]]

## Primary Sources Checked

- Huang et al., "Power of data in quantum machine learning", arXiv:2011.01938 / Nature Communications 12, 2631 (2021).

## Verdict

Major issues. The main idea is right, but the card oversells universality and advantage.

## Line-Anchored Comments

- Lines 5-15: the projected-kernel motivation is good: full fidelity kernels can concentrate and lose generalization.
- Lines 17-23: "contains information about all orders of RDMs and can learn any quantum model with sufficient data" is too strong. The all-order shadow kernel is expressive, but sample complexity, variance, regularization, and train/test distribution still matter.
- Line 23: "quantum hardness is preserved" is also too strong. Measuring local observables on a quantum device can preserve some hard-to-classically-compute features, but projection can also discard precisely the hard feature.
- Lines 36-41: the caveats are good and should be moved closer to the claims they qualify.

## Missing or Suggested Additions

- Add the geometric-difference test assumptions before claiming advantage. A projected kernel is a remedy for concentration, not a proof of useful quantum advantage by itself.

