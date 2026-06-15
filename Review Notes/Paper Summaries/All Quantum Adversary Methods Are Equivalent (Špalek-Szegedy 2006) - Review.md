# Review: All Quantum Adversary Methods Are Equivalent (Spalek-Szegedy 2006)

Source note: [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]

## Primary Sources Checked

- Spalek and Szegedy, "All quantum adversary methods are equivalent", Theory of Computing 2006.

## Verdict

Major wording issue. The equivalence/certificate-barrier summary is mostly correct for positive adversary methods, but the caveat that the adversary method "fails completely" for exact and zero-error quantum complexity is misleading.

## Line-Anchored Comments

- Add a top-level title.
- Lines 19-23: accurate high-level description: this paper unifies the then-known positive adversary formulations and identifies the certificate-complexity barrier.
- Lines 31-43: good spectral-adversary presentation. Be explicit that the matrix is nonnegative/positive-weight in the relevant formulations.
- Lines 76-88: useful certificate-complexity discussion. The total-function bound is central and should stay prominent.
- Line 113: correct, but say "positive adversary" rather than "adversary method" if the note later discusses negative weights.
- Line 114: too strong. A bounded-error lower bound is automatically a lower bound for exact and zero-error algorithms, so the method does not "fail completely" for those models. What is true is that this positive adversary framework is designed for bounded-error query lower bounds and does not characterize exact/zero-error complexity.
- Line 116: good caveat. The negative-weight adversary is outside this paper and removes the certificate barrier.

## Missing or Suggested Additions

- Add a small terminology box: positive adversary, weighted adversary, spectral adversary, Kolmogorov method, negative/general adversary. The current prose uses "all adversary methods" in a way that can accidentally include later negative-weight results.

