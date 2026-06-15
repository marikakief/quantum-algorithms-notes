# Review: Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024)

Source note: [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]]

## Primary Sources Checked

- Schmidhuber, O'Donnell, Kothari, and Babbush, "Quartic quantum speedups for planted inference," Phys. Rev. X 15, 021077 (2025), arXiv:2406.19378.

## Verdict

Minor issues. The note explains the Kikuchi/guiding-state mechanism well, but it sometimes states framework-relative speedups as if they were unconditional speedups over all classical algorithms. The qubit-count and resource-estimate statements also need tighter qualifiers.

## Line-Anchored Comments

- Lines 24-30: the "quartic speedup" should be labeled as relative to Kikuchi/Sum-of-Squares-style classical algorithms under the Alice-theorem framework. The caveat appears later, but it belongs in the opening.
- Line 24: "only `tilde O(log n)` qubits" hides dependence on `ell` and representation registers. Say `polylog(n)` for fixed `ell`, or `tilde O(ell log n)` when `ell` is explicit.
- Line 26: "even against hypothetical improved classical algorithms within the Kikuchi framework" is too broad. It is true for classical guarantees expressible as an Alice theorem with the relevant `ell`, not for arbitrary future classical algorithms that exploit the same planted structure differently.
- Lines 40-45: good high-level algorithm sketch. Add that the phase-estimation/reflection cost includes repeated sparse Hamiltonian simulation of the Kikuchi graph; this is where the large Toffoli estimates enter.
- Lines 57-65: the two-factor speedup table is useful, but the first row should not imply amplitude amplification alone explains the classical-to-quantum comparison. The key comparison is classical explicit vector/matrix work versus quantum sparse-Hamiltonian spectral access.
- Lines 67-71: "classical complexity stays exponential throughout" is too strong. It is the known/classical-Kikuchi-style approach that stays expensive; the paper does not rule out a new implicit classical spectral method.
- Lines 77-89: good result summary. For the concrete example, distinguish "ratio of exponents" from "speedup exponent": `n^32` versus `n^10` is a speedup of `n^22`, while `32/10` is an exponent ratio.
- Line 89 and line 111: comparing the coarse `10^15`-`10^16` Toffoli estimate to early FeMoCo reductions is speculative. Useful context, but not evidence that similar reductions will occur here.
- Lines 107-113: the caveats are strong. Move the first caveat up near the headline result.

## Missing or Suggested Additions

- Add a status line: "Theorem: quantum speedup relative to Kikuchi-method classical algorithm; not known: lower bound against all classical algorithms for planted noisy kXOR."

