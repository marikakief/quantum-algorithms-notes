# Review: Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022)

Source note: [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]

## Primary Sources Checked

- Zhao, Zhou, Shaw, Li, Childs, "Hamiltonian Simulation with Random Inputs," PRL 129, 270502 (2022) / arXiv:2111.04773.

## Verdict

Minor issues. The note is accurate and pedagogically strong. It correctly separates worst-case operator-norm simulation from average-case input-state guarantees.

## Line-Anchored Comments

- Lines 13-23: the Frobenius-norm framing is correct. Add that the gain depends on the nested-commutator spectrum; it is not automatically a `sqrt(d)` improvement for every Hamiltonian.
- Lines 27-37: good explanation of the 1-design assumption. It is worth stating explicitly that the guarantee is over the input ensemble, not for every state in the ensemble.
- Lines 41-79: the product-formula bounds are well summarized. Keep the condition inherited by the interference theorem close to the theorem statement.
- Lines 81-95: the Taylor-series discussion is useful. The note correctly points out that the gate-count impact is mild because Taylor/LCU already has logarithmic precision dependence.
- Lines 101-124: the application tables are clear. Mark them as asymptotic scaling examples rather than complete resource estimates.
- Line 161: the cross-link to "Faster Quantum Simulation by Randomization (Berry-Childs-Su-Wang-Wiebe 2020)" looks like a placeholder or non-existent vault target; verify before publishing.

## Missing or Suggested Additions

- Add one sentence contrasting this with fixed-state/entanglement-dependent follow-up work: average-case over a 1-design is a distributional guarantee, while later work asks when a particular entangled input behaves average-case-like on local error supports.
