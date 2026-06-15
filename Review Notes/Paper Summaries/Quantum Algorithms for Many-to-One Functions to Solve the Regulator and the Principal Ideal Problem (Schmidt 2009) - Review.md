# Review: Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009)

Source note: [[Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) — Paper Notes]]

## Primary Sources Checked

- Schmidt, "Quantum Algorithms for many-to-one Functions to Solve the Regulator and the Principal Ideal Problem", arXiv:0912.4807 (2009).

## Verdict

Minor issues. This is a careful review note and already flags the main suspicious runtime notation. The improvements are mostly presentation: separate the regulator and PIP period lattices more cleanly.

## Line-Anchored Comments

- Lines 11-21: good problem and resource framing. The narrow-regulator convention is important.
- Lines 23-30: the contribution is well stated as a lower-qubit refinement, not a new complexity class.
- Lines 36-60: the positive-`a` reduced-form spacing fact is the core number-theoretic trick. Keep the `ln 2` versus `1/sqrt(Delta)` contrast visible.
- Lines 66-80: the many-to-one regulator function is explained well. The bounded run-length variation is exactly the condition that makes QFT sampling survive.
- Lines 84-104: the Regulator-Dual sample form and success probability are useful.
- Lines 120-145: the PIP lattice basis description is dense but accurate enough. Add a short sentence explaining why two-dimensional Fourier sampling is needed here.
- Lines 162 and 208: good catch on `O(polylog(log Delta))`. Keep this as a caution until checked against the thesis; polynomial in `log Delta` is the sensible reading.
- Lines 182-190 and 209: the non-certifying "not principal" output caveat is important and should be retained.

## Missing or Suggested Additions

- Add a small side-by-side of Hallgren's one-to-one/weakly periodic function versus Schmidt's many-to-one function, emphasizing what changes in the Fourier proof.

