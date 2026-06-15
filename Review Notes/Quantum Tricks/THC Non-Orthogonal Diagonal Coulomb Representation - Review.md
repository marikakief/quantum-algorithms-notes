# Review: THC Non-Orthogonal Diagonal Coulomb Representation

Source note: [[THC Non-Orthogonal Diagonal Coulomb Representation]]

## Primary Sources Checked

- Lee et al., arXiv:2011.03494 / PRX Quantum 2, 030305 (2021).

## Verdict

Major issues. The card captures the important idea, but it states the diagonalization and lambda ordering too literally. The non-orthogonal basis is an implementation device inside an LCU/block encoding, not a simple global basis change.

## Line-Anchored Comments

- Line 7: "transforms ... into a diagonal two-body operator" needs qualification. Because the THC orbitals are non-orthogonal, this is not a standard orthonormal second-quantized basis transformation.
- Lines 11-18: the THC tensor expression and orbitals are good.
- Lines 19-23: the note should say the block encoding realizes diagonal-like `n_mu n_nu` actions by rotating into term-dependent THC orbital directions and rotating back. It should not imply a single globally valid number-operator basis with a harmless overlap matrix.
- Line 25: `lambda_zeta <= lambda_DF in general` is too strong unless quoting a specific theorem with assumptions. The safer statement is that the paper's optimized THC constructions often yield comparable or smaller norms in the benchmarked cases, and direct naive THC can be worse.
- Line 25: "information content `Gamma = O(N^2)` regardless of how `N` grows" assumes THC rank `M=O(N)`. That empirical assumption should be stated in the same sentence.
- Lines 37-39: the `~O(N)` per-step and `~O(N lambda_zeta / epsilon)` total statements are the key correct takeaway, subject to the rank/precision caveats.
- Line 44: this caveat is good. Move part of it earlier.

## Missing or Suggested Additions

- Add a "non-orthogonal does not mean nonunitary SELECT" warning. The actual oracle must still be unitary; the non-orthogonal representation is compiled through controlled unitary rotations and diagonal operations.

