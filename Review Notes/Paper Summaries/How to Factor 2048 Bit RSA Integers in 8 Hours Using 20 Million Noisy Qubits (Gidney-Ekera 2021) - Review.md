# Review: How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021)

Source note: [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]

## Primary Sources Checked

- Gidney and Ekera, "How to factor 2048 bit RSA integers in 8 hours using 20 million noisy qubits", arXiv: https://arxiv.org/abs/1905.09749
- Quantum journal page: https://quantum-journal.org/papers/q-2021-04-15-433/

## Verdict

Minor issues. This is a strong resource-estimation summary. The main correction is a notation slip in the finite-field DLP statement.

## Line-Anchored Comments

- Line 14: finite-field DLP group notation

  Use `Z_p^*` or `F_p^*`, not `Z_N^*` with `N` prime. In the rest of the note `N` means RSA modulus, so line 14 creates a notation collision.

- Lines 18-24: RSA to short discrete log

  Correct for the Gidney-Ekera convention. The note appropriately uses `y = g^{N+1}` and `d = log_g y = p+q` under the high-probability condition that the order is larger than `p+q`.

- Lines 40-56: exponent length

  Good. This is the point of using the Ekerå-Håstad-style reduction: exponent length drops from the vanilla order-finding scale to about `1.5n + O(1)` in the RSA case.

- Lines 76 onward: coset representation / approximate modular arithmetic

  Good, but the error model deserves a pointer. The note should explicitly say these approximations are accounted for by trace-distance/error-budget bounds, not just asserted as harmless.

- Lines 187-189: 5.1 hours versus 8 hours

  Good caveat. The public headline includes retry risk and system assumptions; the per-run wall-clock estimate alone is not the same quantity.

- Line 205: trace-distance bound

  Correct in shape. Keep the distinction between logical circuit error, approximation error, and physical/noise/retry accounting; they are different layers of the paper.

- Lines 211-218: physical resource table

  Useful, but table values should be source-labeled if copied from the paper. This is exactly the kind of note readers will quote.

## Missing or Suggested Additions

- Add the abstract-circuit formula from the journal abstract near the top: `3n + 0.002 n lg n` logical qubits, `0.3 n^3 + 0.0005 n^3 lg n` Toffolis, and `500 n^2 + n^2 lg n` measurement depth.
