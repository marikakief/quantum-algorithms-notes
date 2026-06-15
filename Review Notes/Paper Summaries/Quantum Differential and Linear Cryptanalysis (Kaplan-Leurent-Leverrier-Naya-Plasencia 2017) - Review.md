# Review: Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017)

Source note: [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]

## Primary Sources Checked

- Kaplan, Leurent, Leverrier, and Naya-Plasencia, "Quantum Differential and Linear Cryptanalysis," arXiv:1510.05836v3; IACR Transactions on Symmetric Cryptology 2017.

## Verdict

Minor issues. The note is careful and useful. It correctly separates Q1 and Q2 access, and it explains why truncated differentials do not inherit a full quadratic Grover speedup.

## Line-Anchored Comments

- Lines 20-28: excellent model split. Keep Q1/Q2 visible in every later complexity statement, because the data-complexity conclusions change completely between them.
- Lines 31-33: good framing. The paper is mainly about accounting, and that is exactly why it matters.
- Lines 37-45: the setup/checking Grover template is a useful abstraction. It should not be simplified to "Grover over keys" because several checks are nested.
- Lines 49-67: the simple differential distinguisher is accurately summarized. The lower-bound intuition depends on reducing to unstructured search; keep the known-characteristic assumption attached.
- Lines 71-130: the last-round attack formulas are dense but readable. The definitions of `h_out`, `Delta_fin`, and `k_out` should always be kept near the formulas.
- Lines 131-153: strong discussion of truncated differentials. The element-distinctness/collision structure is the reason for the less-than-quadratic speedup.
- Lines 186-234: the linear-cryptanalysis section is good. The distinction between bias estimation by quantum counting and key completion by Grover is important.
- Lines 249-282: the LAC/KLEIN examples are valuable because they show constants and attack choice can reverse the classical ranking.
- Lines 295-300: caveats are appropriate. Add that finding characteristics/linear approximations is outside the algorithmic scope.

## Missing or Suggested Additions

- Add a compact "best classical attack need not be best quantum attack" callout with the LAC/KLEIN examples.
- Add a warning that Q2 coherent access is a strong model assumption, not the default deployed-cipher attack interface.

