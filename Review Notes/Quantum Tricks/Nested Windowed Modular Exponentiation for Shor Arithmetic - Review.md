# Review: Nested Windowed Modular Exponentiation for Shor Arithmetic

Source note: [[Nested Windowed Modular Exponentiation for Shor Arithmetic]]

## Primary Sources Checked

- Gidney and Ekera, "How to factor 2048 bit RSA integers in 8 hours using 20 million noisy qubits", arXiv: https://arxiv.org/abs/1905.09749
- Quantum journal page: https://quantum-journal.org/papers/q-2021-04-15-433/

## Verdict

Minor issues. The card captures the design pattern, but several formulas need parameter/source labels because this construction is a physical-resource-optimized composition, not a clean asymptotic theorem in isolation.

## Line-Anchored Comments

- Lines 12-16: two-level windowing

  Good. The card correctly separates exponent-window fusion from multiplication-window fusion.

- Lines 17-29: lookup-addition pipeline

  Good high-level description. Mention that this is bound to QROM plus measurement-based uncomputation and to the coset/carry-runway arithmetic context; outside that context the cost model changes.

- Lines 31-37: lookup-addition count

  Needs a source equation/table pointer. The formula is too specific to leave uncited, and parameters such as `c_sep` and the hidden lower-order term need definitions before the equation.

- Lines 47-53: per-lookup Toffoli cost

  Same issue. The formula is meaningful only under the paper's chosen adder, QROM, padding, and uncomputation conventions. Mark it as the Gidney-Ekera cost model, not a generic windowed-exponentiation cost.

- Lines 55-58: window-size caveat

  Good. It is important that the optimized windows are small constants in their physical model; blindly increasing window size grows lookup cost exponentially.

## Missing or Suggested Additions

- Add the baseline it improves over: unwindowed modular exponentiation applies `n_e` controlled modular multiplications, each decomposed into controlled additions, before QROM/window/carry-runway reductions.
