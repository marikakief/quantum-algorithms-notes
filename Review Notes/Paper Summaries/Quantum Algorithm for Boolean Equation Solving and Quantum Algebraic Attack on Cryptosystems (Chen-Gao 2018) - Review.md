# Review: Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018)

Source note: [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]

## Primary Sources Checked

- Chen and Gao, "Quantum algorithms for Boolean equation solving and quantum algebraic attack on cryptosystems", arXiv:1712.06239.

## Verdict

Minor issues. This is a careful note and it already says the most important thing: the cryptanalytic interpretation is condition-number dependent. Only a few precision and presentation fixes are needed.

## Line-Anchored Comments

- Lines 17-29: good problem setup and good warning that the cryptanalytic application depends on Macaulay condition numbers.
- Lines 33-39: the assessment is appropriately skeptical. Keep this near the top.
- Lines 43-58: the HHL/Macaulay setup is readable. Consider distinguishing query complexity of the sparse matrix oracle from full fault-tolerant implementation cost.
- Lines 77-96: the complete-solving-degree discussion is useful and should stay; it is one of the genuinely algebraic parts of the construction.
- Lines 103-117: good explanation of pseudo-solution states.
- Lines 121-139: the monomial-measurement extraction procedure is clear, but emphasize that this is a probabilistic extraction of one Boolean solution, not a solution-set description.
- Lines 190-198: the two Boolean-system runtimes are both listed. Add one sentence explaining how the `\tilde O((n^{3.5}+T_F^{3.5})...)` shorthand follows from the explicit `(n+T_F)^{2.5}(n+7T_F)` expression.
- Lines 201-211: table values are fine as long as every row keeps the `c kappa^2` factor. Do not quote the exponents alone.
- Lines 224-234: excellent caveats, especially the later Ding-Gheorghiu-Gilyen-Hallgren-Li limitation.

## Missing or Suggested Additions

- Add a one-line "not a practical AES break" warning in the key-results section, not only in limits/caveats.

