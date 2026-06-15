# Review: Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018)

Source note: [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]

## Primary Sources Checked

- Babbush et al., "Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity", arXiv:1805.03662 / PRX 8, 041015 (2018).
- Cross-checked against later QROAM terminology in Berry et al. 2019.

## Verdict

Major issues. The summary captures the primitives well, but two resource statements are materially wrong: the Hubbard scaling line double-counts `N`, and the caveat about qubits drops the system register.

## Line-Anchored Comments

- Line 32: the "one million superconducting qubits and a few hours" claim is source-specific and should be tied to the paper's surface-code assumptions. It is not a general consequence of the asymptotic theorem.
- Line 42: `lambda = O(N^2)` should be framed for the plane-wave-dual electronic-structure Hamiltonian under the paper's assumptions. "Typical molecular problems at fixed electron density" is too broad because arbitrary Gaussian bases and first-quantized plane waves have different `lambda` behavior.
- Line 50: the parenthetical about garbage registers is delicate. In qubitization the standard-form state can include structured garbage only if PREPARE and its inverse are used consistently in the reflection. Do not suggest that tracing out garbage is harmless.
- Lines 97-107: the theorem statement is good. Keep this as the canonical total T-count expression.
- Line 127: with `lambda = 2Nt + Nu/2`, Theorem 2 gives `O(N lambda / epsilon) = O(N^2(t+u)/epsilon)`, not `O(N^2 lambda / epsilon)`.
- Line 137: "combined oracle call" qubits as `O(log(lambda N / epsilon))` omits the `N` system qubits. It may be an ancilla-only count, but the table does not say so.
- Line 164: "The qubit count here is `O(log(lambda N/epsilon))`" is wrong for total logical qubits. The algorithm still has the second-quantized system register. The source theorem's ancilla count is logarithmic; the total includes `N` system qubits.
- Line 191: the reference row labels the CI paper as "Babbush et al. (2018), QST, 1506.01029", which is fine, but avoid making it look like it is the same paper as this PRX article.

## Missing or Suggested Additions

- Separate three resource categories in the note: system qubits, ancilla qubits, and physical surface-code qubits. Several current claims conflate the first two.
- Add a short warning that QROM's `4L-4` cost is a T-gate/Toffoli accounting convention tied to the primitives used in this paper; later QROAM papers usually speak in Toffoli counts.

