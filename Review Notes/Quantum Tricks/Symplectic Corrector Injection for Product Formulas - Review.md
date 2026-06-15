# Review: Symplectic Corrector Injection for Product Formulas

Source note: [[Symplectic Corrector Injection for Product Formulas]]

## Primary Sources Checked

- Bagherimehrab et al., arXiv:2409.08265: https://arxiv.org/abs/2409.08265

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Line 7: "one or more orders of magnitude" should be replaced with "one or more orders in the local-error expansion" or "large constant-factor/error-order improvements." Magnitude and asymptotic order are different.
- Lines 11-19: the corrector mechanism is correctly summarized.
- Lines 21-25: the boundary-only cost identity is the key insight and is stated clearly.
- Lines 35-40: the compilation caveat is important; correctors are not free unless the commutator exponentials can be compiled cheaply enough.
- Line 41: the non-perturbed caveat is helpful and should stay.

## Missing or Suggested Additions

- Add a side-by-side with THRIFT: both exploit a perturbative split, but CPF corrects BCH errors while THRIFT changes frame.
